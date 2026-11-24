**Scores Conversion Script**

Run this code from the folder containing the CSV. 
```
install.packages("jsonlite")
# ============================================================
# READ SCORES.CSV
# ============================================================

scores <- utils::read.csv(
  "scores.csv",
  stringsAsFactors = FALSE,
  check.names = FALSE,
  colClasses = "character",
  fileEncoding = "UTF-8-BOM",
  strip.white = TRUE,
  na.strings = "",
  comment.char = ""
)

# Preserve identifiers as text and trim column headings.
names(scores) <- trimws(names(scores))

if (nrow(scores) == 0L) {
  stop(
    "scores.csv contains no data rows.",
    call. = FALSE
  )
}

if (anyDuplicated(names(scores))) {
  stop(
    "scores.csv contains duplicate column headings.",
    call. = FALSE
  )
}


# ============================================================
# REQUIRED COLUMNS
# ============================================================

required_columns <- c(
  "NodeKey",
  "QID",
  "ChoicePosition",
  "EnterpriseImpact",
  "CriticalRigor",
  "StrategicDecisiveness",
  "StakeholderTrust",
  "Resourcefulness",
)

missing_columns <- setdiff(
  required_columns,
  names(scores)
)

if (length(missing_columns) > 0L) {
  stop(
    "Missing columns: ",
    paste(missing_columns, collapse = ", "),
    call. = FALSE
  )
}


# ============================================================
# HELPERS
# ============================================================

optional_text <- function(value) {
  if (
    length(value) != 1L ||
    is.na(value)
  ) {
    return("")
  }

  trimws(as.character(value))
}


required_text <- function(
  value,
  column_name,
  csv_row
) {
  text <- optional_text(value)

  if (!nzchar(text)) {
    stop(
      "scores.csv row ",
      csv_row,
      ": ",
      column_name,
      " cannot be blank.",
      call. = FALSE
    )
  }

  text
}


validate_integer <- function(
  value,
  minimum,
  maximum,
  column_name,
  csv_row
) {
  text <- optional_text(value)

  number <- suppressWarnings(
    as.numeric(text)
  )

  if (
    !nzchar(text) ||
    !is.finite(number) ||
    number != floor(number) ||
    number < minimum ||
    number > maximum
  ) {
    stop(
      "scores.csv row ",
      csv_row,
      ": ",
      column_name,
      " must be a whole number from ",
      minimum,
      " through ",
      maximum,
      ". Found: '",
      text,
      "'.",
      call. = FALSE
    )
  }

  as.integer(number)
}


# ============================================================
# BUILD SCORING CONFIGURATION
# ============================================================

scores_by_qid <- list()

for (i in seq_len(nrow(scores))) {
  row <- scores[i, , drop = FALSE]
  csv_row <- i + 1L

  qid <- required_text(
    row$QID[[1]],
    "QID",
    csv_row
  )

  node_key <- required_text(
    row$NodeKey[[1]],
    "NodeKey",
    csv_row
  )

  if (!grepl("^QID[0-9]+$", qid)) {
    stop(
      "scores.csv row ",
      csv_row,
      ": invalid QID '",
      qid,
      "'.",
      call. = FALSE
    )
  }

  if (!grepl("^[1-4]\\.[1-5]$", node_key)) {
    stop(
      "scores.csv row ",
      csv_row,
      ": NodeKey must use a value such as ",
      "1.1, 2.4, or 4.5. Found: '",
      node_key,
      "'.",
      call. = FALSE
    )
  }

  position <- as.character(
    validate_integer(
      row$ChoicePosition[[1]],
      minimum = 1,
      maximum = 5,
      column_name = "ChoicePosition",
      csv_row = csv_row
    )
  )

  if (is.null(scores_by_qid[[qid]])) {
    scores_by_qid[[qid]] <- list()
  }

  if (!is.null(scores_by_qid[[qid]][[position]])) {
    stop(
      "scores.csv row ",
      csv_row,
      ": duplicate scoring record for ",
      qid,
      ", ChoicePosition ",
      position,
      ".",
      call. = FALSE
    )
  }

  # Validate each dimension using the current 0–4 rubric.
  read_score <- function(column_name) {
    validate_integer(
      row[[column_name]][[1]],
      minimum = 0,
      maximum = 4,
      column_name = column_name,
      csv_row = csv_row
    )
  }



 
  # Optional display text.
  title <- if ("Title" %in% names(scores)) {
    optional_text(row$Title[[1]])
  } else {
    ""
  }

  body <- if ("Body" %in% names(scores)) {
    optional_text(row$Body[[1]])
  } else {
    ""
  }

  scores_by_qid[[qid]][[position]] <- list(
    nodeKey = node_key,

    enterpriseImpact = read_score(
      "EnterpriseImpact"
    ),

    criticalRigor = read_score(
      "CriticalRigor"
    ),

    strategicDecisiveness = read_score(
      "StrategicDecisiveness"
    ),

    stakeholderTrust = read_score(
      "StakeholderTrust"
    ),

    resourcefulness = read_score(
      "Resourcefulness"
    ),

   
    title = title,
    body = body
  )
}


# ============================================================
# VALIDATE QUESTION AND NODE MAPPINGS
# ============================================================

node_to_qid <- list()

for (qid in names(scores_by_qid)) {
  choices <- scores_by_qid[[qid]]

  # Each question must contain exactly positions 1–5.
  if (!setequal(names(choices), as.character(1:5))) {
    stop(
      qid,
      " must contain exactly five choices ",
      "with positions 1 through 5.",
      call. = FALSE
    )
  }

  node_keys <- unique(
    vapply(
      choices,
      function(choice) choice$nodeKey,
      character(1)
    )
  )

  if (length(node_keys) != 1L) {
    stop(
      qid,
      " contains inconsistent NodeKey values.",
      call. = FALSE
    )
  }

  node_key <- node_keys[[1]]

  # The scoring ledger and profile calculation assume
  # one scored QID per NodeKey.
  if (!is.null(node_to_qid[[node_key]])) {
    stop(
      "NodeKey ",
      node_key,
      " is assigned to both ",
      node_to_qid[[node_key]],
      " and ",
      qid,
      ".",
      call. = FALSE
    )
  }

  node_to_qid[[node_key]] <- qid

  # Keep choice records in numerical order.
  scores_by_qid[[qid]] <- choices[
    as.character(1:5)
  ]
}


# ============================================================
# WRITE SCORES.JSON
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

  scoresByQID = scores_by_qid
)

jsonlite::write_json(
  output,
  path = "scores.json",
  auto_unbox = TRUE,
  pretty = TRUE,
  na = "null"
)


# ============================================================
# REPORT RESULTS
# ============================================================

cat(
  "Created scores.json\n",
  "Version:", output$version,
  "\nScored questions:", length(scores_by_qid),
  "\nChoice records:",
  sum(lengths(scores_by_qid)),
  "\n"
)
```
