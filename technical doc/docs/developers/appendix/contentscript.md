**Content Conversion Script**

```

library(jsonlite)

# ============================================================
# HELPERS
# ============================================================

# Convert a value to valid UTF-8.
#
# The CSV is expected to be UTF-8, but the fallback handles
# characters commonly introduced by Excel or Word.
clean_utf8 <- function(value) {
  if (
    length(value) == 0L ||
    is.na(value)
  ) {
    return(NA_character_)
  }

  value <- as.character(value)

  # First, test whether the value is already valid UTF-8.
  converted <- iconv(
    value,
    from = "UTF-8",
    to = "UTF-8",
    sub = NA
  )

  # If not, try the common Windows/Excel encoding.
  if (is.na(converted)) {
    converted <- iconv(
      value,
      from = "windows-1252",
      to = "UTF-8",
      sub = ""
    )
  }

  Encoding(converted) <- "UTF-8"

  converted
}


# Clean every character column in a data frame.
clean_data_frame_utf8 <- function(data) {
  data[] <- lapply(
    data,
    function(column) {
      if (!is.character(column)) {
        return(column)
      }

      vapply(
        column,
        clean_utf8,
        character(1)
      )
    }
  )

  data
}


# Read a CSV, trying UTF-8 first and then Windows-1252.
read_configuration_csv <- function(filename) {
  data <- tryCatch(
    read.csv(
      filename,
      stringsAsFactors = FALSE,
      check.names = TRUE,
      fileEncoding = "UTF-8-BOM",
      na.strings = c("", "NA")
    ),
    error = function(error) {
      message(
        filename,
        " could not be read as UTF-8; ",
        "trying Windows-1252."
      )

      read.csv(
        filename,
        stringsAsFactors = FALSE,
        check.names = TRUE,
        fileEncoding = "windows-1252",
        na.strings = c("", "NA")
      )
    }
  )

  clean_data_frame_utf8(data)
}


validate_columns <- function(
  data,
  required_columns,
  filename
) {
  missing_columns <- setdiff(
    required_columns,
    names(data)
  )

  if (length(missing_columns) > 0L) {
    stop(
      paste0(
        filename,
        " is missing these exact columns: ",
        paste(
          missing_columns,
          collapse = ", "
        ),
        "\nR found these columns instead: ",
        paste(
          names(data),
          collapse = ", "
        )
      )
    )
  }
}


required_text <- function(
  value,
  field_name,
  row_number,
  filename
) {
  value <- clean_utf8(value)

  if (
    length(value) == 0L ||
    is.na(value)
  ) {
    stop(
      paste0(
        filename,
        " row ",
        row_number,
        " has a blank or unreadable ",
        field_name,
        "."
      )
    )
  }

  value <- trimws(value)

  if (!nzchar(value)) {
    stop(
      paste0(
        filename,
        " row ",
        row_number,
        " has a blank ",
        field_name,
        "."
      )
    )
  }

  value
}


optional_text <- function(value) {
  value <- clean_utf8(value)

  if (
    length(value) == 0L ||
    is.na(value)
  ) {
    return("")
  }

  trimws(value)
}


is_active <- function(value) {
  value <- clean_utf8(value)

  if (is.na(value)) {
    return(FALSE)
  }

  normalized <- tolower(
    trimws(
      as.character(value)
    )
  )

  normalized %in% c(
    "1",
    "true",
    "yes",
    "y",
    "active"
  )
}


is_truthy <- function(value) {
  value <- clean_utf8(value)

  if (is.na(value)) {
    return(FALSE)
  }

  normalized <- tolower(
    trimws(
      as.character(value)
    )
  )

  normalized %in% c(
    "1",
    "true",
    "yes",
    "y",
    "on"
  )
}


# ============================================================
# IMPORT FILES
# ============================================================

curveballs <- read_configuration_csv(
  "curveballs.csv"
)

archetypes <- read_configuration_csv(
  "archetypes.csv"
)

settings <- read_configuration_csv(
  "settings.csv"
)

node_content <- read_configuration_csv(
  "nodetitle_body.csv"
)


# ============================================================
# REQUIRED COLUMNS
# ============================================================

# check.names = TRUE converts spaces and punctuation in column
# names to dots.
required_curveball_columns <- c(
  "CurveballID",
  "PoolNumber",
  "Category",
  "PoolPosition",
  "Title",
  "Body",
  "Active"
)

required_archetype_columns <- c(
  "ArchetypeID",
  "Title",
  "Philosophy",
  "Core.Strengths",
  "Inherent.Strategic.Trade.off",
  "Midpoint.Diagnostic.Text",
  "Final.Debrief.Text",
  "Active"
)

required_setting_columns <- c(
  "Setting",
  "Value",
  "Active"
)

required_node_content_columns <- c(
  "QID",
  "Title",
  "Body"
)


# ============================================================
# VALIDATE COLUMNS
# ============================================================

validate_columns(
  curveballs,
  required_curveball_columns,
  "curveballs.csv"
)

validate_columns(
  archetypes,
  required_archetype_columns,
  "archetypes.csv"
)

validate_columns(
  settings,
  required_setting_columns,
  "settings.csv"
)

validate_columns(
  node_content,
  required_node_content_columns,
  "nodetitle_body.csv"
)


# ============================================================
# FILTER ACTIVE RECORDS
# ============================================================

curveballs <- curveballs[
  vapply(
    curveballs$Active,
    is_active,
    logical(1)
  ),
  ,
  drop = FALSE
]

archetypes <- archetypes[
  vapply(
    archetypes$Active,
    is_active,
    logical(1)
  ),
  ,
  drop = FALSE
]

settings <- settings[
  vapply(
    settings$Active,
    is_active,
    logical(1)
  ),
  ,
  drop = FALSE
]


# ============================================================
# BUILD SETTINGS
# ============================================================

setting_names <- vapply(
  settings$Setting,
  optional_text,
  character(1)
)

development_rows <- settings[
  tolower(setting_names) ==
    "developmentmode",
  ,
  drop = FALSE
]

if (nrow(development_rows) != 1L) {
  stop(
    paste(
      "settings.csv must contain exactly one active",
      "DevelopmentMode row."
    )
  )
}

development_mode <- is_truthy(
  development_rows$Value[[1]]
)

settings_record <- list(
  developmentMode = development_mode
)


# ============================================================
# BUILD CURVEBALLS
# ============================================================

if (nrow(curveballs) == 0L) {
  stop(
    "curveballs.csv contains no active curveballs."
  )
}

pool_numbers <- suppressWarnings(
  as.integer(curveballs$PoolNumber)
)

pool_positions <- suppressWarnings(
  as.integer(curveballs$PoolPosition)
)

if (any(is.na(pool_numbers))) {
  stop(
    "curveballs.csv contains an invalid PoolNumber."
  )
}

if (any(is.na(pool_positions))) {
  stop(
    "curveballs.csv contains an invalid PoolPosition."
  )
}

curveball_categories <- vapply(
  curveballs$Category,
  optional_text,
  character(1)
)

curveballs <- curveballs[
  order(
    pool_numbers,
    curveball_categories,
    pool_positions
  ),
  ,
  drop = FALSE
]

curveball_records <- lapply(
  seq_len(nrow(curveballs)),
  function(i) {
    row <- curveballs[
      i,
      ,
      drop = FALSE
    ]

    list(
      curveballID = required_text(
        row[["CurveballID"]][[1]],
        "CurveballID",
        i + 1L,
        "curveballs.csv"
      ),

      poolNumber = as.integer(
        row[["PoolNumber"]][[1]]
      ),

      category = required_text(
        row[["Category"]][[1]],
        "Category",
        i + 1L,
        "curveballs.csv"
      ),

      poolPosition = as.integer(
        row[["PoolPosition"]][[1]]
      ),

      title = required_text(
        row[["Title"]][[1]],
        "Title",
        i + 1L,
        "curveballs.csv"
      ),

      body = required_text(
        row[["Body"]][[1]],
        "Body",
        i + 1L,
        "curveballs.csv"
      ),

      active = TRUE
    )
  }
)


# ============================================================
# VALIDATE AND BUILD ARCHETYPES
# ============================================================

expected_archetype_ids <- c(
  "archetype_a",
  "archetype_b",
  "archetype_c",
  "archetype_d",
  "archetype_e"
)

if (nrow(archetypes) != 5L) {
  stop(
    paste0(
      "Expected 5 active archetypes but found ",
      nrow(archetypes),
      "."
    )
  )
}

actual_archetype_ids <- vapply(
  seq_len(nrow(archetypes)),
  function(i) {
    required_text(
      archetypes$ArchetypeID[[i]],
      "ArchetypeID",
      i + 1L,
      "archetypes.csv"
    )
  },
  character(1)
)

duplicate_archetype_ids <- unique(
  actual_archetype_ids[
    duplicated(actual_archetype_ids)
  ]
)

if (length(duplicate_archetype_ids) > 0L) {
  stop(
    paste0(
      "Duplicate ArchetypeID values: ",
      paste(
        duplicate_archetype_ids,
        collapse = ", "
      )
    )
  )
}

missing_archetype_ids <- setdiff(
  expected_archetype_ids,
  actual_archetype_ids
)

if (length(missing_archetype_ids) > 0L) {
  stop(
    paste0(
      "Missing expected ArchetypeID values: ",
      paste(
        missing_archetype_ids,
        collapse = ", "
      )
    )
  )
}

unexpected_archetype_ids <- setdiff(
  actual_archetype_ids,
  expected_archetype_ids
)

if (length(unexpected_archetype_ids) > 0L) {
  stop(
    paste0(
      "Unexpected ArchetypeID values: ",
      paste(
        unexpected_archetype_ids,
        collapse = ", "
      )
    )
  )
}

archetypes <- archetypes[
  match(
    expected_archetype_ids,
    actual_archetype_ids
  ),
  ,
  drop = FALSE
]

archetype_records <- lapply(
  seq_len(nrow(archetypes)),
  function(i) {
    row <- archetypes[
      i,
      ,
      drop = FALSE
    ]

    list(
      archetypeID = required_text(
        row[["ArchetypeID"]][[1]],
        "ArchetypeID",
        i + 1L,
        "archetypes.csv"
      ),

      title = required_text(
        row[["Title"]][[1]],
        "Title",
        i + 1L,
        "archetypes.csv"
      ),

      philosophy = required_text(
        row[["Philosophy"]][[1]],
        "Philosophy",
        i + 1L,
        "archetypes.csv"
      ),

      coreStrengths = required_text(
        row[["Core.Strengths"]][[1]],
        "Core Strengths",
        i + 1L,
        "archetypes.csv"
      ),

      inherentStrategicTradeoff =
        required_text(
          row[[
            "Inherent.Strategic.Trade.off"
          ]][[1]],
          "Inherent Strategic Trade-off",
          i + 1L,
          "archetypes.csv"
        ),

      midpointDiagnosticText =
        required_text(
          row[[
            "Midpoint.Diagnostic.Text"
          ]][[1]],
          "Midpoint Diagnostic Text",
          i + 1L,
          "archetypes.csv"
        ),

      finalDebriefText =
        required_text(
          row[[
            "Final.Debrief.Text"
          ]][[1]],
          "Final Debrief Text",
          i + 1L,
          "archetypes.csv"
        ),

      active = TRUE
    )
  }
)


# ============================================================
# BUILD NODE CONTENT BY QID
# ============================================================

if (nrow(node_content) == 0L) {
  stop(
    "nodetitle_body.csv contains no rows."
  )
}

nodes_by_qid <- list()

for (i in seq_len(nrow(node_content))) {
  row <- node_content[
    i,
    ,
    drop = FALSE
  ]

  qid <- required_text(
    row[["QID"]][[1]],
    "QID",
    i + 1L,
    "nodetitle_body.csv"
  )

  if (!is.null(nodes_by_qid[[qid]])) {
    stop(
      paste0(
        "nodetitle_body.csv contains duplicate QID: ",
        qid
      )
    )
  }

  nodes_by_qid[[qid]] <- list(
    title = required_text(
      row[["Title"]][[1]],
      "Title",
      i + 1L,
      "nodetitle_body.csv"
    ),

    body = required_text(
      row[["Body"]][[1]],
      "Body",
      i + 1L,
      "nodetitle_body.csv"
    )
  )
}


# ============================================================
# BUILD OUTPUT
# ============================================================

output <- list(
  version = format(
    Sys.time(),
    "%Y-%m-%d-%H%M%S"
  ),

  generatedAt = format(
    Sys.time(),
    "%Y-%m-%dT%H:%M:%SZ",
    tz = "UTC"
  ),

  settings = settings_record,

  curveballs = curveball_records,

  archetypes = archetype_records,

  nodesByQID = nodes_by_qid
)


# ============================================================
# WRITE CONTENT.JSON
# ============================================================

jsonlite::write_json(
  output,
  path = "content.json",
  auto_unbox = TRUE,
  pretty = TRUE,
  na = "null"
)


# ============================================================
# REPORT RESULTS
# ============================================================

cat(
  "Created content.json\n",
  "Development mode:",
  development_mode,
  "\nCurveballs:",
  length(curveball_records),
  "\nArchetypes:",
  length(archetype_records),
  "\nNode questions:",
  length(nodes_by_qid),
  "\n"
)
```

