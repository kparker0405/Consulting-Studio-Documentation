**Scores Conversion Script**

Run this code from the folder containing the CSV. You may need to install jsonlite by running:
```
install.packages("jsonlite")
```

```
scores <- read.csv(
  "scores.csv",
  stringsAsFactors = FALSE,
  check.names = FALSE,
  fileEncoding = "UTF-8-BOM"
)

required_columns <- c(
  "NodeKey",
  "QID",
  "ChoicePosition",
  "EnterpriseImpact",
  "CriticalRigor",
  "StrategicDecisiveness",
  "StakeholderTrust",
  "Resourcefulness",
  "StateField",
  "StateValue",
  "Title",
  "Body"
)

missing_columns <- setdiff(
  required_columns,
  names(scores)
)

if (length(missing_columns) > 0) {
  stop(
    paste(
      "Missing columns:",
      paste(missing_columns, collapse = ", ")
    )
  )
}

scores_by_qid <- list()

for (row_number in seq_len(nrow(scores))) {
  row <- scores[row_number, ]

  qid <- trimws(
    as.character(row$QID)
  )

  position <- as.character(
    as.integer(row$ChoicePosition)
  )

  if (is.null(scores_by_qid[[qid]])) {
    scores_by_qid[[qid]] <- list()
  }

  state_field <- if (
    is.na(row$StateField)
  ) {
    ""
  } else {
    trimws(
      as.character(row$StateField)
    )
  }

  state_value <- if (
    is.na(row$StateValue)
  ) {
    ""
  } else {
    trimws(
      as.character(row$StateValue)
    )
  }

  title <- if (
    is.na(row$Title)
  ) {
    ""
  } else {
    trimws(
      as.character(row$Title)
    )
  }

  body <- if (
    is.na(row$Body)
  ) {
    ""
  } else {
    trimws(
      as.character(row$Body)
    )
  }

  scores_by_qid[[qid]][[position]] <- list(
    nodeKey = trimws(
      as.character(row$NodeKey)
    ),

    enterpriseImpact = as.numeric(
      row$EnterpriseImpact
    ),

    criticalRigor = as.numeric(
      row$CriticalRigor
    ),

    strategicDecisiveness = as.numeric(
      row$StrategicDecisiveness
    ),

    stakeholderTrust = as.numeric(
      row$StakeholderTrust
    ),

    resourcefulness = as.numeric(
      row$Resourcefulness
    ),

    stateField = state_field,
    stateValue = state_value,

    title = title,
    body = body
  )
}

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

  scoresByQID = scores_by_qid
)

jsonlite::write_json(
  output,
  path = "scores.json",
  auto_unbox = TRUE,
  pretty = TRUE,
  na = "null"
)
```
