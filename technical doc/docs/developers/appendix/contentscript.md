**Content Conversion Script**

```
library(jsonlite)

# ============================================================
# IMPORT FILES
# ============================================================

curveballs <- read.csv(
  "curveballs.csv",
  stringsAsFactors = FALSE,
  check.names = FALSE,
  fileEncoding = "UTF-8-BOM",
  na.strings = c("", "NA")
)

archetypes <- read.csv(
  "archetypes.csv",
  stringsAsFactors = FALSE,
  check.names = FALSE,
  fileEncoding = "UTF-8-BOM",
  na.strings = c("", "NA")
)

settings <- read.csv(
  "settings.csv",
  stringsAsFactors = FALSE,
  check.names = FALSE,
  fileEncoding = "UTF-8-BOM",
  na.strings = c("", "NA")
)

node_content <- read.csv(
  "nodetitle_body.csv",
  stringsAsFactors = FALSE,
  check.names = FALSE,
  fileEncoding = "UTF-8-BOM",
  na.strings = c("", "NA")
)


# ============================================================
# VALIDATE COLUMNS
# ============================================================

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
  "Core Strengths",
  "Inherent Strengths",
  "Midpoint Diagnostic Text",
  "Final Debrief Text",
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
        " is missing: ",
        paste(
          missing_columns,
          collapse = ", "
        )
      )
    )
  }
}


# ============================================================
# HELPERS
# ============================================================

is_active <- function(value) {
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

required_text <- function(
  value,
  field_name,
  row_number,
  filename
) {
  if (
    length(value) == 0L ||
    is.na(value)
  ) {
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

  cleaned <- trimws(
    as.character(value)
  )

  if (!nzchar(cleaned)) {
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

  cleaned
}


# ============================================================
# VALIDATE FILE COLUMNS
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

development_rows <- settings[
  tolower(
    trimws(settings$Setting)
  ) == "developmentmode",
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

curveballs <- curveballs[
  order(
    as.integer(curveballs$PoolNumber),
    curveballs$Category,
    as.integer(curveballs$PoolPosition)
  ),
  ,
  drop = FALSE
]

curveball_records <- lapply(
  seq_len(nrow(curveballs)),
  function(i) {
    row <- curveballs[i, ]

    list(
      curveballID = required_text(
        row$CurveballID[[1]],
        "CurveballID",
        i + 1L,
        "curveballs.csv"
      ),

      poolNumber = as.integer(
        row$PoolNumber[[1]]
      ),

      category = required_text(
        row$Category[[1]],
        "Category",
        i + 1L,
        "curveballs.csv"
      ),

      poolPosition = as.integer(
        row$PoolPosition[[1]]
      ),

      title = required_text(
        row$Title[[1]],
        "Title",
        i + 1L,
        "curveballs.csv"
      ),

      body = required_text(
        row$Body[[1]],
        "Body",
        i + 1L,
        "curveballs.csv"
      ),

      active = TRUE
    )
  }
)


# ============================================================
# BUILD ARCHETYPES
# ============================================================

expected_archetype_ids <- c(
  "archetype_egs",
  "archetype_cas",
  "archetype_pis",
  "archetype_yvm",
  "archetype_ua"
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
      "archetypes.csv contains duplicate IDs: ",
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

unexpected_archetype_ids <- setdiff(
  actual_archetype_ids,
  expected_archetype_ids
)

if (length(missing_archetype_ids) > 0L) {
  stop(
    paste0(
      "archetypes.csv is missing: ",
      paste(
        missing_archetype_ids,
        collapse = ", "
      )
    )
  )
}

if (length(unexpected_archetype_ids) > 0L) {
  stop(
    paste0(
      "archetypes.csv has unexpected IDs: ",
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
    row <- archetypes[i, ]

    list(
      archetypeID = required_text(
        row$ArchetypeID[[1]],
        "ArchetypeID",
        i + 1L,
        "archetypes.csv"
      ),

      title = required_text(
        row$Title[[1]],
        "Title",
        i + 1L,
        "archetypes.csv"
      ),

      philosophy = required_text(
        row$Philosophy[[1]],
        "Philosophy",
        i + 1L,
        "archetypes.csv"
      ),

      coreStrengths = required_text(
        row[["Core Strengths"]][[1]],
        "Core Strengths",
        i + 1L,
        "archetypes.csv"
      ),

      inherentStrengths = required_text(
        row[["Inherent Strengths"]][[1]],
        "Inherent Strengths",
        i + 1L,
        "archetypes.csv"
      ),

      midpointDiagnosticText = required_text(
        row[["Midpoint Diagnostic Text"]][[1]],
        "Midpoint Diagnostic Text",
        i + 1L,
        "archetypes.csv"
      ),

      finalDebriefText = required_text(
        row[["Final Debrief Text"]][[1]],
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
  row <- node_content[i, ]

  qid <- required_text(
    row$QID[[1]],
    "QID",
    i + 1L,
    "nodetitle_body.csv"
  )

  title <- required_text(
    row$Title[[1]],
    "Title",
    i + 1L,
    "nodetitle_body.csv"
  )

  body <- required_text(
    row$Body[[1]],
    "Body",
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
    title = title,
    body = body
  )
}


# ============================================================
# WRITE CONTENT.JSON
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

jsonlite::write_json(
  output,
  path = "content.json",
  auto_unbox = TRUE,
  pretty = TRUE,
  na = "null"
)

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
)
```

