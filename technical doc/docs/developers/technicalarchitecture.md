**Technical Architecture**
## Platform components
|Component|Function|
|---|---|
|Qualtrics|Survey delivery, responses, embedded data, daily flow|
|Qualtrics Files Library| Holds static JSON configuration files|
|Browser session stroage|Temporarily caches configuration files|
|JavaScript|Assigns curveballs, score choices, and inserts dynamic content|
|Google Sheets| Authoring source for simulation configuration|
|R conversion scripts| Convert spreadsheet CSV exports into JSON|
|Apps Scripts|Release schedule only|

### Choice order

Each Qualtrics decision question contains five choices corresponding to positions `1–5`.

The position must match the scoring configuration:

| Displayed choice | Configuration position |
|---|---:|
| First | 1 |
| Second | 2 |
| Third | 3 |
| Fourth | 4 |
| Fifth | 5 |

**Choice randomization must remain disabled** unless the scoring architecture is redesigned.


## Configuration files
scores.json

Contains:
* QID mappings
* NodeKey mappings
* Choice positions
* Five-dimensional score effects
* Optional strategic-state changes

content.json

Contains:
* Development-mode setting
* Curveball library
* Final archetype content
* Version metadata

Release schedule

Release times are stored in [Google Sheets](https://docs.google.com/spreadsheets/d/1OtIUH017O9_c3Iu6NgrQPSEVaZbZBHqCsXRnLo81jzY/edit?usp=sharing) 

Contains:
* Level
* The data and time the level will be unlocked
* The timezone

## Browser storage
The simulation uses sessionStorage to cache:
* migsScoreConfig
* migsScoreConfigVersion
* migsCurveballConfig
* migsArchetypeConfig
* migsContentVersion
* migsDeveopmentMode

Configuration-refresh pages reload these files before each daily level. This supports respondent returning after cllsing the browser or switching devices. 
