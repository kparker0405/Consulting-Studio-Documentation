**Content Conversion Script**

```
library(jsonlite)

# ============================================================
# HELPERS
# ============================================================

# Read the entire file using one consistent encoding.
# Try UTF-8 first, then Windows-1252.
# Do not silently discard unreadable characters.
read_configuration_csv <- function(filename) {
  if (!file.exists(filename)) {
    stop("File not found: ", filename, call. = FALSE)
  }

  file_size <- file.info(filename)$size

  if (is.na(file_size) || file_size == 0) {
    stop(filename, " is empty.", call. = FALSE)
  }

  raw_bytes <- readBin(
    filename,
    what = "raw",
    n = file_size
  )

  # Remove a UTF-8 byte-order mark, if present.
  if (
    length(raw_bytes) >= 3L &&
    identical(
      raw_bytes[1:3],
      as.raw(c(239, 187, 191))
    )
  ) {
    raw_bytes <- raw_bytes[-(1:3)]
  }

  original_text <- rawToChar(raw_bytes)

  text <- iconv(
    original_text,
    from = "UTF-8",
    to = "UTF-8",
    sub = NA
  )

  if (is.na(text)) {
    message(
      filename,
      ": UTF-8 decoding failed; trying Windows-1252."
    )

    text <- iconv(
      original_text,
      from = "windows-1252",
      to = "UTF-8",
      sub = NA
    )
  }

  if (is.na(text)) {
    stop(
      filename,
      " contains unreadable characters. ",
      "Save it as CSV UTF-8 and try again.",
      call. = FALSE
    )
  }

  connection <- textConnection(text)
  on.exit(close(connection), add = TRUE)

  # Read every column as text to preserve IDs and formatting.
  data <- utils::read.csv(
    connection,
    stringsAsFactors = FALSE,
    check.names = TRUE,
    colClasses = "character",
    strip.white = TRUE,
    na.strings = "",
    comment.char = ""
  )

  if (anyDuplicated(names(data))) {
    stop(
      filename,
      " contains duplicate column names.",
      call. = FALSE
    )
  }

  # Record source row numbers before filtering or sorting.
  data$.csv_row <- seq_len(nrow(data)) + 1L

  data
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
      filename,
      " is missing these columns: ",
      paste(missing_columns, collapse = ", "),
      "\nR found: ",
      paste(names(data), collapse = ", "),
      call. = FALSE
    )
  }

  invisible(TRUE)
}


required_text <- function(
  value,
  field_name,
  row_number,
  filename
) {
  if (
    length(value) != 1L ||
    is.na(value) ||
    !nzchar(trimws(as.character(value)))
  ) {
    stop(
      filename,
      " row ",
      row_number,
      " has a blank ",
      field_name,
      ".",
      call. = FALSE
    )
  }

  trimws(as.character(value))
}


optional_text <- function(value) {
  if (
    length(value) != 1L ||
    is.na(value)
  ) {
    return("")
  }

  trimws(as.character(value))
}


is_active <- function(value) {
  tolower(optional_text(value)) %in% c(
    "1", "true", "yes", "y", "active", "on"
  )
}


is_truthy <- function(value) {
  tolower(optional_text(value)) %in% c(
    "1", "true", "yes", "y", "on"
  )
}


validate_unique_ids <- function(
  ids,
  field_name,
  filename
) {
  duplicate_ids <- unique(
    ids[duplicated(ids)]
  )

  if (length(duplicate_ids) > 0L) {
    stop(
      filename,
      " contains duplicate ",
      field_name,
      " values: ",
      paste(duplicate_ids, collapse = ", "),
      call. = FALSE
    )
  }

  invisible(TRUE)
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
# NORMALIZE ALTERNATIVE ARCHETYPE HEADINGS
# ============================================================

# check.names = TRUE converts headings such as
# "Core Strengths" into "Core.Strengths".
#
# Accept either:
# "Inherent Strategic Trade-off"
# "Inherent Trade-off"

if (
  !"Inherent.Strategic.Trade.off" %in% names(archetypes) &&
  "Inherent.Trade.off" %in% names(archetypes)
) {
  names(archetypes)[
    names(archetypes) == "Inherent.Trade.off"
  ] <- "Inherent.Strategic.Trade.off"
}


# ============================================================
# VALIDATE REQUIRED COLUMNS
# ============================================================

validate_columns(
  curveballs,
  c(
    "CurveballID",
    "Category",
    "Title",
    "Body",
    "Active"
  ),
  "curveballs.csv"
)

validate_columns(
  archetypes,
  c(
    "ArchetypeID",
    "Title",
    "Philosophy",
    "Core.Strengths",
    "Inherent.Strategic.Trade.off",
    "Midpoint.Diagnostic.Text",
    "Final.Debrief.Text",
    "Active"
  ),
  "archetypes.csv"
)

validate_columns(
  settings,
  c(
    "Setting",
    "Value",
    "Active"
  ),
  "settings.csv"
)

validate_columns(
  node_content,
  c(
    "QID",
    "Title",
    "Body"
  ),
  "nodetitle_body.csv"
)


# ============================================================
# FILTER ACTIVE RECORDS
# ============================================================

curveballs <- curveballs[
  vapply(curveballs$Active, is_active, logical(1)),
  ,
  drop = FALSE
]

archetypes <- archetypes[
  vapply(archetypes$Active, is_active, logical(1)),
  ,
  drop = FALSE
]

settings <- settings[
  vapply(settings$Active, is_active, logical(1)),
  ,
  drop = FALSE
]

# Node content may optionally include an Active column.
if ("Active" %in% names(node_content)) {
  node_content <- node_content[
    vapply(node_content$Active, is_active, logical(1)),
    ,
    drop = FALSE
  ]
}


# ============================================================
# BUILD SETTINGS
# ============================================================

setting_names <- vapply(
  settings$Setting,
  optional_text,
  character(1)
)

development_rows <- settings[
  tolower(setting_names) == "developmentmode",
  ,
  drop = FALSE
]

if (nrow(development_rows) != 1L) {
  stop(
    "settings.csv must contain exactly one active ",
    "DevelopmentMode row.",
    call. = FALSE
  )
}

development_value <- tolower(
  required_text(
    development_rows$Value[[1]],
    "DevelopmentMode Value",
    development_rows$.csv_row[[1]],
    "settings.csv"
  )
)

if (
  !development_value %in% c(
    "true", "false",
    "1", "0",
    "yes", "no",
    "y", "n",
    "on", "off"
  )
) {
  stop(
    "DevelopmentMode must be TRUE or FALSE ",
    "(or an equivalent supported value).",
    call. = FALSE
  )
}

development_mode <- is_truthy(
  development_value
)

settings_record <- list(
  developmentMode = development_mode
)


# ============================================================
# BUILD CURVEBALLS
# ============================================================

if (nrow(curveballs) == 0L) {
  stop(
    "curveballs.csv contains no active curveballs.",
    call. = FALSE
  )
}

# IMPORTANT:
# Preserve CSV row order.
# Group assignment depends on the curveball array order.
# Do not sort by Category, Title, or CurveballID.

curveball_records <- lapply(
  seq_len(nrow(curveballs)),
  function(i) {
    row <- curveballs[i, , drop = FALSE]
    csv_row <- row$.csv_row[[1]]

    list(
      curveballID = required_text(
        row$CurveballID[[1]],
        "CurveballID",
        csv_row,
        "curveballs.csv"
      ),

      category = required_text(
        row$Category[[1]],
        "Category",
        csv_row,
        "curveballs.csv"
      ),

      title = required_text(
        row$Title[[1]],
        "Title",
        csv_row,
        "curveballs.csv"
      ),

      body = required_text(
        row$Body[[1]],
        "Body",
        csv_row,
        "curveballs.csv"
      ),

      active = TRUE
    )
  }
)

curveball_ids <- vapply(
  curveball_records,
  function(record) record$curveballID,
  character(1)
)

validate_unique_ids(
  curveball_ids,
  "CurveballID",
  "curveballs.csv"
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

actual_archetype_ids <- vapply(
  seq_len(nrow(archetypes)),
  function(i) {
    required_text(
      archetypes$ArchetypeID[[i]],
      "ArchetypeID",
      archetypes$.csv_row[[i]],
      "archetypes.csv"
    )
  },
  character(1)
)

validate_unique_ids(
  actual_archetype_ids,
  "ArchetypeID",
  "archetypes.csv"
)

missing_ids <- setdiff(
  expected_archetype_ids,
  actual_archetype_ids
)

unexpected_ids <- setdiff(
  actual_archetype_ids,
  expected_archetype_ids
)

if (
  length(missing_ids) > 0L ||
  length(unexpected_ids) > 0L
) {
  stop(
    "archetypes.csv must contain the five IDs ",
    "archetype_a through archetype_e.",
    "\nMissing: ",
    paste(missing_ids, collapse = ", "),
    "\nUnexpected: ",
    paste(unexpected_ids, collapse = ", "),
    call. = FALSE
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
    row <- archetypes[i, , drop = FALSE]
    csv_row <- row$.csv_row[[1]]

    read_field <- function(column_name) {
      required_text(
        row[[column_name]][[1]],
        column_name,
        csv_row,
        "archetypes.csv"
      )
    }

    list(
      archetypeID = read_field(
        "ArchetypeID"
      ),

      title = read_field(
        "Title"
      ),

      philosophy = read_field(
        "Philosophy"
      ),

      coreStrengths = read_field(
        "Core.Strengths"
      ),

      inherentStrategicTradeoff = read_field(
        "Inherent.Strategic.Trade.off"
      ),

      midpointDiagnosticText = read_field(
        "Midpoint.Diagnostic.Text"
      ),

      finalDebriefText = read_field(
        "Final.Debrief.Text"
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
    "nodetitle_body.csv contains no active rows.",
    call. = FALSE
  )
}

nodes_by_qid <- list()

for (i in seq_len(nrow(node_content))) {
  row <- node_content[i, , drop = FALSE]
  csv_row <- row$.csv_row[[1]]

  qid <- required_text(
    row$QID[[1]],
    "QID",
    csv_row,
    "nodetitle_body.csv"
  )

  if (!is.null(nodes_by_qid[[qid]])) {
    stop(
      "nodetitle_body.csv contains duplicate QID: ",
      qid,
      call. = FALSE
    )
  }

  nodes_by_qid[[qid]] <- list(
    title = required_text(
      row$Title[[1]],
      "Title",
      csv_row,
      "nodetitle_body.csv"
    ),

    # Body may be blank when the title contains the full prompt.
    body = optional_text(
      row$Body[[1]]
    )
  )
}


# ============================================================
# BUILD AND WRITE OUTPUT
# ============================================================

output <- list(
  version = format(
    Sys.time(),
    "%Y-%m-%d-%H%M%S",
    tz = "UTC"
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


# ============================================================
# REPORT RESULTS
# ============================================================

cat(
  "Created content.json\n",
  "Version:", output$version,
  "\nDevelopment mode:", development_mode,
  "\nCurveballs:", length(curveball_records),
  "\nArchetypes:", length(archetype_records),
  "\nNode questions:", length(nodes_by_qid),
  "\n"
)


```
