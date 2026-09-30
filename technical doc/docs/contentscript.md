## 27. Technical Appendix E: Content Conversion Script

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
  "Body",
  "Active"
)

required_setting_columns <- c(
  "Setting",
  "Value",
  "Active"
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
      curveballID = trimws(
        as.character(
          row$CurveballID[[1]]
        )
      ),

      poolNumber = as.integer(
        row$PoolNumber[[1]]
      ),

      category = trimws(
        as.character(
          row$Category[[1]]
        )
      ),

      poolPosition = as.integer(
        row$PoolPosition[[1]]
      ),

      title = trimws(
        as.character(
          row$Title[[1]]
        )
      ),

      body = trimws(
        as.character(
          row$Body[[1]]
        )
      ),

      active = TRUE
    )
  }
)


# ============================================================
# BUILD ARCHETYPES
# ============================================================

archetype_records <- lapply(
  seq_len(nrow(archetypes)),
  function(i) {
    row <- archetypes[i, ]

    list(
      archetypeID = trimws(
        as.character(
          row$ArchetypeID[[1]]
        )
      ),

      title = trimws(
        as.character(
          row$Title[[1]]
        )
      ),

      body = trimws(
        as.character(
          row$Body[[1]]
        )
      ),

      active = TRUE
    )
  }
)


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

  archetypes = archetype_records
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
  "\n"
)
```

