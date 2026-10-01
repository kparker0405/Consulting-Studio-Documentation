### 14. Technical Architecture
## 14.1 Platform components
|Component|Function|
|---|---|
|Qualtrics|Survey delivery, responses, embedded data, daily flow|
|Qualtrics Files Library| Holds static JSON configuration files|
|Browser session stroage|Temporarily caches configuration files|
|JavaScript|Assigns curveballs, score choices, and inserts dynamic content|
|Google Sheets| Authoring source for simulation configuration|
|R conversion scripts| Convert spreadsheet CSV exports into JSON|
|Apps Scripts|Release schedule only|
## 14.2 Configuration files
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

## 14.3 Browser storage
The simulation uses sessionStorage to cache:
* migsScoreConfig
* migsScoreConfigVersion
* migsCurveballConfig
* migsArchetypeConfig
* migsContentVersion
* migsDeveopmentMode

Configuration-refresh pages reload these files before each daily level. This supports respondent returning after cllsing the browser or switching devices. 

## 15. Qualtrics Embedded Data
|Field category| Title| Notes|
|---|---|---|
|Participation and curveballs|__js_GroupMode|  |
 | |__js_TeamID| |
 | |__js_TeamSeed| |
 | |__js_Curveball1ID| |
 | |__js_Curveball2ID| |
 | |__js_Curveball3ID| |
 | |__js_Curveball3Category| |
 | |__js_CurveballStatus| |
 |Configuration |__js_DevelopmentMode| |
 | |__js_ScoringConfigVersion| |
 | |__js_ContentConfigVersion| |
 |Scores |__js_EnterpriseImpactScore| |
 | |__js_CriticalRigorScoreS| |
 | |__js_StrategicDecisivenessScore| |
 | |__js_StakeholderTrustScore| |
 | |__js_ResourcefulnessScore| |
 | |__js_ScoredNodes|Persistent list of nodes already scored. |
 |Strategic State |__js_InitialStrategy| |
 | |__js_MidpointStrategy| |
 | |__js_CrisisStrategyResponse| |
 | |__js_FinalStrategy| |
 |Released |__js_Level2Unlocked| |
 | |__js_Level3Unlocked| |
 | |__js_Level4Unlocked| |
 |Final outcome |__js_FinalArchetype| |
 |Debugging |__js_Level2UnlockTimestamp| |
 | |__js_Level3UnlockTimestamp| |
 | |__js_Level4UnlockTimestamp| |
 | |__js_ArchetypeCalculated| |





## 16. Development Mode
Development mode is configured in content.json.

**Development mode enabled**

When DevelopmentMode = TRUE, maintainers may see:
* Score totals
* Configuration versions
* Current QID and NodeKey
* Selected choice position
* Assigned curveball IDs
* Curveball category
* Final score thresholds
* Technical diagnosticc messages

**Development mode disabled**

When DevelopmentMode = FALSE, students see:
* Narrative content
* Decision questions
* Decision logs
* Recaps
* Curveballs
* Final outcome

They do not see internal scoring or technical details.

## 17. Release-Schedule Methodology
Levels are released according to this schedule:
|Level| Unlocked data/time| Time zone|
|---|---|---|
|2| [Date/time] | America/Detroit|
|3| [Date/time] | America/Detroit|
|4| [Date/time] | America/Detroit|

Release logic uses the Google Apps Script server time.

Development mode bypasses release restrictions.

**Checkpoint behavior**

Before release:
* The next level remains inaccessible.
* Students see the planned release time.
* Students may close the response and return later.

After release:
* The next level becomes available.
* Configuration files are refreshed.
* The next recap and curveball are displayed.

## 18. Content-Management Workflow
### 18.1 Editing scores
1. Open the scoring workbook
2. Edit the Scores sheet.
3. Export or save scores.csv.
4. Run the R conversion script.
5. Generate a new scores.json.
6. Upload it to the Qualtrics Files Library.
7. Update the preload URL if the file URL changed.
8. Begin a fresh test response.
9. Verify the displayed configuration version.
10. Publish the survey.

### 18.2 Editing curveballs or archetypes
1. Edit the curveballs.csv, archetypes.csv, or settings.csv. 
2. Run the content-conversion script.
3. Generate a new content.json.
4. Upload it to the Qualtrics Files Library.
5. Update the file URL if necessary.
6. Test curveball and display endings.
7. Publish the survey.

### 18.3 Editing the release schedule
1. Edit the release schedule Google Sheet. 
It will automatically update. 

### 18.4 Versioning convention
Recommended version format:

YYYY-MM-DD-HHMMSS

Record production versions here:
|Date|Survey version|Scores version|Content version|Editor|Summary|
|--|--|--|--|--|--|
|[Date] | [Version] | [Version] | [Version] | [Name] | [Change] | 
## Technical Appendix

* [Survey Flow](surveyflow.md)
* [File Schemas](fileschemas.md)
* [Configuration Urls](configurls.md)
* [Scores Conversion Script](scorescript.md)
* [Content Conversion Script](contentscript.md)

## Pre Launch Check List
* [Testing Methodology](testing.md)
