<iframe src="https://docs.google.com/spreadsheets/d/e/2PACX-1vQOXZuRoKPkChXAChDMq6wDVBkD60gqzAkI-FOXRTGpaeZo_fMbVvYLql7p8oFJJUXS8IY6BUmIdx_w/pubhtml?widget=true&amp;headers=false"></iframe>
[Follow this link to edit](https://docs.google.com/spreadsheets/d/184a11AemXY3gg1jEQXWjXGkamxT9nsvRcbMDx_Motlw/edit?usp=sharing)

## 19. Testing Methodology
### 19.1 Required prelaunched tests
|Test| Expected result| Status| 
| --| --| --|
| Content configuration loads| 20 curveballs and 4 archetypes| [ ] |
| Scoring configuration loads| 20 QIDS and 5 choices each| [ ]| 
| Development mode ON|Diagnostics are visible| [ ]|
|Development mode OFF| Diagnostics are hidden|[ ]|
| Solo mode| Random curveballs assigned| [ ]|
|Group mode| Deterministic curveballs assigned| [ ]|
|Same Team ID repeated| Same assignments appear| [ ]|
|Different Team IDs| Balanced assignments appear| [ ]|
|Choice changed before Next| Score recalculates correctly| [ ]|
|Page revisited| Score is not counted twice| [ ]|
|Level 2 refresh| Configuration is restored| [ ]|
|Level 3 refresh| Configuration is restored| [ ]|
|Level 4 refresh| Configuration is restored| [ ]|
| Release before deadline| Nexxt level remains locked| [ ]|
|Release after deadline| Next level becomes available| [ ]|
|Final archetype| Correct ending is displayed| [ ]|
|Response export| Required embedded fields are present| [ ]|

### 19.2 Archetype tests

Create at least one test path intended to produce each outcome:

|Archetype| Test path/reference| Expected| Actual| Pass?|
|--|--|--|--|--|
|Regional Powerhouse| [Path] |[Scores] | [Scores] | [ ]|
|Viral Spectacle| [Path] |[Scores] | [Scores] | [ ]|
|Safe Hometown Club| [Path] |[Scores] | [Scores] | [ ]|
|Unaligned Agency| [Path] |[Scores] | [Scores] | [ ]|

### 19.3 Browser testing
Test using:
* Chrome
* Firefox
* Safari
* Edge
