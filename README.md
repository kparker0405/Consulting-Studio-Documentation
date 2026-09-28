# HEEEEHAW Beyond the Grandstand: Qualtrics Simulation Methodology and Technical Reference

**Course:** BA 600 001 – Consulting Studio  
**School:** University of Michigan – Stephen M. Ross School of Business  
**Project partners:** Office of Action-Based Learning and Office of Digital Education  
**Document owner:** Kat Parker - ODE  
**Faculty lead:** Andy Wicklund  
**Technical lead:** Kat Parker  
**Simulation version:** Version 1  
**Last updated:** 09/25/2026  
**Status:** Draft

---

## 1. Document Purpose

This document explains the instructional design, simulation methodology, technical architecture, scoring system, content-management process, and operating procedures for the **Beyond the Grandstand: Scaling the Manitou Island Ghosts** simulation.

It is intended for:

- Faculty members
- Course administrators
- Action-based learning staff
- Digital education staff
- Future simulation maintainers
- Other project stakeholders

This document is designed to answer the following questions:

1. What is the simulation intended to teach?
2. How does the four-level experience work?
3. How are team decisions scored?
4. How are curveballs assigned?
5. How does the simulation remain consistent for group participants?
6. Which parts of the simulation are managed in Qualtrics?
7. Which content is managed outside Qualtrics?
8. How are updates tested and published?
9. What data are collected?
10. What should future maintainers know?

---

## 2. Executive Summary

**Beyond the Grandstand** is a multi-day, team-based consulting simulation hosted in Qualtrics. Students act as consultants to the Manitou Island Ghosts, an independent minor-league baseball organization seeking to become a major regional attraction.

The simulation operates as a rule-based game master. It:

- Presents a common business case
- Guides teams through sequential decision nodes
- Introduces changing business conditions
- Assigns curveballs
- Tracks hidden performance dimensions
- Records team recommendations and reflections
- Provides recaps across multiple simulation levels
- Produces a final strategic archetype

Decisions, scoring effects, curveball assignments, branching, and outcomes are determined by predefined rules and configuration files.

### High-level design

| Component | Approach |
|---|---|
| Delivery platform | Qualtrics |
| Participation modes | Solo and group |
| Number of levels | 4 |
| Decision nodes per level | 5 |
| Total scored nodes | 20 |
| Choices per node | 5 |
| Curveball pools | 3 |
| Scoring dimensions | 5 |
| Student score visibility | Hidden |
| Narrative content source | Qualtrics-hosted JSON |
| Scoring source | Qualtrics-hosted JSON |
| Release schedule | Google Sheet/API |
| Final outcomes | 4 archetypes |

---

## 3. Instructional Context

### 3.1 Course context

The simulation takes place during the opening week of BA 600. It prepares Master of Management students for a sponsored action-based learning engagement by allowing them to practice problem framing, decision-making under uncertainty, stakeholder management, and collaborative consulting behaviors.

### 3.2 Student audience

- Program: All Winter Master of Management students
- Approximate enrollment: [Number]
- Typical prior business experience: Limited
- Typical team size: [Number]
- Number of teams: [Number]
- Relevant accessibility or scheduling considerations: [Description]

### 3.3 Why a simulation is used

[Explain why the learning objectives cannot be addressed as effectively through lecture or a static written case alone.]

Example:

> A static case allows students to analyze a stable set of facts. This simulation instead requires teams to make decisions before all relevant information is available and then adapt when external conditions change. This better approximates the ambiguity, stakeholder complexity, and iterative judgment involved in a consulting engagement.

---

## 4. Learning Objectives

The simulation supports the following learning objectives.

### 4.1 Defining and solving problems to improve enterprise performance

Students will:

- Define complex business problems, create and test hypotheses, analyze data, and deliver actionable recommendations utilizing frameworks learned in core courses. 
- Drive change and innovation within an organizational context.


### 4.2 Making decisions under uncertainty and ambiguity

Students will:

- Plan, execute, control, and close projects successfully. 
- Navigate ambiguity, develop risk mitigation strategies and adapt to unforeseen changes swiftly and effectively.
- Experience personal growth through intentional exposure to challenging opportunities strengthening resilience through the process.



### 4.3 Communicating persuasively to build courage and conviction to act

Students will:

- Prepare and deliver compelling presentations and reports to stakeholders. 
- Develop negotiation skills and build consensus among team members and stakeholders.


### 4.4 Working collaboratively in inclusive teams and with learning partners

Students will:

- Lead diverse teams, manage conflicts, foster collaboration, and deliver shared success across learning partners including sponsors and faculty advisors through establishing and nurturing high-quality professional relationships.

### 4.5 Thinking critically to identify opportunities and deliver impact

Students will:

- Ethically evaluate tradeoffs of business decisions. 
- Be aware of bias and assumptions. 
- Consider the cultural context in which the project is taking place.


---

## 5. Business Case Summary

### 5.1 Organization

The Manitou Island Ghosts (MIGs) are an independent minor league baseball team operating out of Traverse City, Michigan. Playing their home games at a stadium just off the main tourist corridor, the Ghosts enjoy steady, localized fan support. Ticket sales remain stable, covering overhead and keeping the lights on, but the franchise has reached a plateau. Despite Traverse City’s booming summer tourism industry, the Ghosts remain an afterthought for visitors who prioritize wineries, beaches, and dining.

### 5.2 Strategic challenge

The Chief Marketing Officer (CMO) has set an ambitious goal: transform the Ghosts from a casual regional pastime into a primary regional attraction—a must-visit destination akin to the sports-entertainment phenomenon of the Savannah Bananas. The franchise owners want a strategy that drives revenue, increases brand equity, and leverages the region's tourism footprint without alienating their loyal local fan base.

### 5.3 Primary objectives

- Brand Transformation: Shift public perception from a standard minor league ballclub to a premier sports-entertainment destination.
- Economic Impact: Drive higher out-of-town attendance, merchandise sales, and community partnerships.
- Strategic Roadmap: Deliver an actionable, high-ROI marketing and operational plan to the team sponsor and ownership group within a four-phase rollout.


### 5.4 Intentional ambiguity

The case does not provide all information needed to make a risk-free decision. This is intentional.

Students must decide:

- Which evidence is most important
- Which assumptions are acceptable
- When additional analysis is warranted
- How much to adapt after receiving new information
- Which tradeoffs are most important

---

## 6. Simulation Structure

The simulation contains four levels. Each level corresponds to one phase  For the first four days of Consulting Studio, students will experience a new “event.” Each day of class will correspond with ~one week of simulated time to allow for more believable development of the events/case.

| Level | Scheduled day | Phase | Primary learning emphasis |
|---:|---|---|---|
| 1 | Wednesday | Scope definition and initial engagement | Problem framing and research |
| 2 | Thursday | Deep analysis and strategic adaptation | Updating strategy |
| 3 | Friday | Stress testing and crisis navigation | Resilience and implementation |
| 4 | Tuesday | Final recommendation and ownership pitch | Synthesis and persuasion |

### 6.1 Level 1: Scope Definition and Initial Engagement

Students make decisions about:

1. Strategic scope
2. Research and market discovery
3. Use of support assets
4. Stakeholder expectations
5. Initial engagement deliverable

**End-of-level activity:** Wednesday decision log

### 6.2 Level 2: Deep Analysis and Strategic Adaptation

Before Level 2 decisions, each participant or team receives a Tourism and Regional Infrastructure curveball.

Students make decisions about:

1. Responding to the development
2. Strategic positioning
3. [Node 2.3 topic]
4. [Node 2.4 topic]
5. [Node 2.5 topic]

**End-of-level activity:** Thursday decision log

### 6.3 Level 3: Stress Testing and Crisis Navigation

Before Node 3.1, each participant or team receives an Economic and Corporate Landscape curveball.

After Node 3.1, each participant or team receives either:

- A Local Policy and Community Relations curveball, or
- A Competitive Environment and Market Dynamics curveball

Students then complete Nodes 3.2–3.5.

**End-of-level activity:** Friday decision log

### 6.4 Level 4: Final Recommendation and Ownership Pitch

Students consolidate their work into a final recommendation addressing:

1. Final strategy
2. Implementation and governance
3. Financial justification
4. Executive communication
5. Final submission

**End-of-level activity:** Final recommendation and team reflection

---

## 7. Participation Modes

### 7.1 Group mode

In group mode:

- Students enter an assigned Team ID between the numbers 1 and 100. 
- The Team ID is stored as embedded data.
- Curveballs are assigned deterministically.
- The same Team ID always receives the same curveballs.
- One designated scribe should operate the Qualtrics response.
- Other team members should participate through discussion.

### 7.2 Solo mode

In solo mode:

- No Team ID is required.
- Curveballs are selected randomly from the relevant pools.
- The selected assignments are stored in embedded data.
- Assignments remain fixed within that response.

### 7.3 Group consistency

The simulation does not provide simultaneous collaborative editing. The recommended operating procedure is:

> One designated scribe advances the simulation on behalf of the group. The group discusses each decision before the scribe submits it.


---

## 8. Decision-Node Methodology

Each decision node contains five options.

The options are designed to be:

- Professionally plausible
- Defensible under some assumptions
- Associated with meaningful tradeoffs
- More or less appropriate depending on context
- Less vulnerable to obvious answer-test strategies

### 8.1 Choice-design principle

The simulation avoids using obviously poor distractors such as:

- Ignore the development
- Submit incomplete work
- Ask someone else to make the decision
- Omit all financial analysis
- Conceal weaknesses from a stakeholder

Instead, choices generally represent tensions such as:

- Speed versus evidence
- Growth versus resilience
- Control versus stakeholder alignment
- Focus versus optionality
- Innovation versus implementation feasibility
- Immediate action versus staged testing

### 8.2 Choice order

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

---

## 9. Hidden Scoring Methodology

Students do not see their score totals during normal production use.

Each decision affects one or more of five dimensions.

| Dimension | Definition |
|---|---|
| Enterprise Impact | Potential to improve growth, revenue, brand value, or organizational performance |
| Critical Rigor | Quality of analysis, evidence, assumptions, and risk evaluation |
| Strategic Decisiveness | Ability to establish priorities, make recommendations, and maintain appropriate focus |
| Stakeholder Trust | Ability to build credibility, alignment, and professional relationships |
| Resourcefulness | Ability to use available resources, partnerships, pilots, and adaptive approaches effectively |

### 9.1 Score updates

For each selected choice:


```
New cumulative score
=
Previous cumulative score
+
Choice-specific score effect
```

### 9.2 Score range
Score effects currently use:

```
-2 to 2
```
A negative score indicates a tradeoff or risk. It does not necessarily mean that the choice is irrational or professionally indefensible. 

### 9.3 Student visibility
In production mode:
- Students do not see scores.
- Students do not see scoring thresholds.
- Students do not receive "correct" or "incorrect" messages.
- Consequences are communicated through narrative develpoments and the final outcome. 

In development mode:
- Current cumulative scores may be displayed. 
- QIDs, NodeKeys, and selected positions may be displayed. 
- Configuration versions may be displayed. 

---
### Strategic-State Variables
Some decisions establish or updated a named strategic state.
| State variable | Set at | Purpose|
| ----------- | ----------- | -----------|
|InitialStategy | Node 1.1 | Records the initial project direction |     
MidpointStrategy | Node 2.2 | Records the strategy after initial adaptation |
|CrisisStrategyResponse | Node 3.4 | Records the crisis-response posture|
|FinalStrategy | Node 4.1 | Records the final strategic direction |

These variables support:
- Recap text
- Faculty analysis
- Final reflection
- Interpretation of the team's strategic path

---
## 11. Curveball Methodology
### 11.1 Curveball pools
|Pool|Category|Number of events| Placement|
|----|------|-----|----|
|1|Tourism and Regional Infrastructure|5|Before Level 2|
|2| Economic and Corporate Landscape|5|Before Node 3.1|
|3A|Local Policy and Community Relations|5|After Node 3.1|
|3B|Competitive Environment and Market Dynamics|5|After Node 3.1|

### 11.2 Group assignment
Group-mode curveballs are assigned deterministcally using the Team ID. This provides:
- Consistency for all members of the same team
- Balanced assignment across up to 100 groups
- Reproducible assignments
- Comparable but nonidentical team experiences

The formula that designates which curveball is assigned to which group is based off of calculates done to to their team number. 

For Pool 1, each of the five events appears 20 times across Teams 1-100.

For Pool 2, we use a different balanced pattern so its assignment is not identifcal to pool 1, and each of the five events appears 20 times across Teams 1-100.

For Pool 3, odd-numbered teams are designated a curveball from the Community/Policy category, and even-numbered teams is designated a curveball from the Competition/Market category. Each event in the selected Pool 3 category appears 10 times across Teams 1-100. 

### 11.3 Solo assignment
Solo participants receive:
- One random event from Pool 1
- One random event from Pool 2
- One random Pool 3 category
- One random event from the selected Pool 3 category

The selected events are stored in embedded variables and remain fixed for the duration of the simulation. 

### 11.4 Curveball content
Curveball content is maintained outside individual Qualtrics questions. Each curveball contains:
```
CurveballID
PoolNumber (1-3)
Category
PoolPosition (1-5)
Title
Body
Active (0-1)
```
One generic Qualtrics display question inserts the assigned title and body dynamically.
## 12. Final Archetype Methodology
The simulation produces one of four ending archetypes. 
|Archetype| General interpretation|
|-----|-----|
|Regional Powerhouse| Strong enterprise impact, rigor, and stakeholder trust|
|Viral Spectacle| High enterprise impact without equally strong balance in other dimensions|
|Safe Hometown Club| Strong stakeholder trust but less enterprise impact|
|Unaligned Agency|Insufficient strategic alignment, trust, or enterprise impact|
### 12.1 Current decision logic
```
IF Enterprise Impact is high 
AND Critical Rigor is high
AND Stakeholder Trust is high:
    Regional Powerhouse

Else if Enterprise Impact is high:
    Viral Spectacle

Else if Stakeholder Trust is high:
    Safe Hometown Club

Else:
    Unaligned Agency
```
### 12.2 Threshold Calculations
The maximum possible score for a dimension is calculated by:
1. Identifying the highest available score for that dimension at each node.
2. Summing those node-level maximums.
3. Multiplying the total by the threshold percentage.
4. Rounding upward.
The threshold is currently the maximum possible score multiply by 60%.
### 12.3 Ending content
Final archetype titles and descriptions are stored in the simulation content file and dynamically insertde into one Qualtrics outcome question.
## 13. Decision Logs and Recaps
### 13.1 Purpose
Decision logs capture information that multiple-choice selections cannot fully represent:
* Current recommendation
* Primary rationale
* Main uncertainty 
* Team decision process
* Confidence level
### 13.2 Daily decision-log fields
|Field|Purpose|
|---|---|
|Recommendation|Captures the team's current direction|
|Rationale| Captures the strongest supporting reason|
|Uncertainty|Identifies the most important unresolved assumption|
|Alignment|Records how the group reached the decision|
|Confidence|Records confidence on a 1-5 scale|
### 13.3 Recaps
At the start of later levels, Qualtrics displays a concise recap using piped text from earlier decision logs.
Recaps may include:
* Current strategic direction
* Previous recommendation
* Primary uncertainty
* Crisis-response posture
* Conditions that would cause reconsideration
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
|Field category| Title| Data type|
|---|---|---|
|Participation fields|__js_GroupMode |Numeric|
| |__js_TeamID | Numeric|
| | __js_TeamSeed | Numeric|
|Configuration fields | __js_DevelopmentMode| Numeric | 
| | __js_SettingsLoaded|  |
| | __js_ScoringConfigLoaded| |
| | __js_ScoringConfigVersion| |
| | __js_ContentConfigLoaded| | 
| | __js_ContentVersion| |
|Score fields| __js_EnterpriseImpactScore| |
| | __js_CriticalRigorScore| |
| | __js_StrategicDecisivenessScore| | 
| | __js_StakeholderTrustScore| |
| | __js_ResourcefulnessScore | |
| Curveball fields| __js_Curveball1ID| |
| | __js_Curveball2ID | |
| | __js_Curveball3ID| |
| | __js_Curveball3Category| |
| | __js_CurveballStatus| |
|Strategic-state fields|__js_InitialStrategy| |
| | __js_MidpointStrategy| |
| | __js_CrisisStrategyResponse| |
| | __js_FinalStrategy| |
|Final-outcome fields|__js_FinalArchetype| |
| |__js_EnterpriseImpactThreshold| |
| | __js_CriticalRigorThreshold| |
| |__js_StakeholderTrustThreshold| |
| | __js_ArchetypeCalculated| |
|Scoring ledger fields|__js_ScoredNodes| |
| | __js_Scored1_1| |
| | __js_SelectedPosition_1_1| |
| | ----- | |
| | __js_Scored_4_5| |
| | __js_SelectedPosition_4_5| |
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

## 17. Release-Schedule Methodology##
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

## 20. Known Limitations
* Qualtrics is not a simultaneous multiuser collaboration platform.
* One designated scribe must operate the simultation for each team.
* Static JSON edits require file regeneration and re-upload.
* Group identity is based on a Team ID rather than full authentication.

## 21. Recovery Procedures
**Configuration file fails to load.**

1. Confirm the Qualtrics File Library URL works.
2. Open the JSON URL directly.
3. Confirm the JSON is valid.
4. Confirm the required top-level property exists.
5. Confirm the survey uses the current file URL.
6. Begin a new preview response.
7. Check the browser console.

**Curveball does not display**
1. Check CurveballStatus.
2. Check the stored CurveballID.
3. Confirm migsCurveballConfig exists.
4. Confirm the ID appears in content.json.
5. Confirm the curveball display page occurs after assignment. 

**Scores do not update**
1. Confirm migsScoreConfig exists.
2. Confirm the QID appears in scores.json.
3. Confirm the Qualtrics question has five choices.
4. Confirm answer randomization is disabled.
5. Confirm the NodeKey is not already present in ScoredNodes.
6. Confirm score embedded-data fields use the __js_ prefix.

**A level does not unlock**

1. Check the release-schedule record.
2. Confirm the time zone is America/Detroit.
3. Test the release endpoint directly.
4. Confirm the checkpoint uses the correct level.
5. Confirm development-mode behavior.

## 22. Prelaunch Checklist
**Content**

- [ ] All 20 nodes have final wording.
- [ ] Every node has five choices.
- [ ] Choices are plausible and balanced.
- [ ] Scores have been reviewed by faculty.
- [ ] Curveball titles and bodies are final.
- [ ] Archetype titles and bodies are final.
- [ ] Decision-log prompts are final.
- [ ] Recap text is final.

**Configuration**

- [ ] scores.json was regenerated.
- [ ] content.json was regenerated.
- [ ] Both files were uploaded to Qualtrics.
- [ ] Current file URLs are in preload scripts.
- [ ] Development mode is set to FALSE.
- [ ] Release dates are correct.
- [ ] Time zone is correct.
- [ ] Configuration versions are documented.

**Qualtrics**

- [ ] Embedded-data fields are defined.
- [ ] Decision questions are forced response.
- [ ] Choice randomization is disabled.
- [ ] One scored question appears per page.
- [ ] Configuration refresh appears before each level.
- [ ] Curveball assignments are stored.
- [ ] Final archetype is stored.
- [ ] Incomplete-response retention covers the full assignment.
- [ ] No early page reaches End of Survey.
- [ ] Published version matches the tested version.

**Testing**

- [ ] Solo path passed.
- [ ] Group path passed.
- [ ] Resume path passed.
- [ ] All curveball displays passed.
- [ ] All archetypes passed.
- [ ] Development mode is hidden.
- [ ] Response export was reviewed.
- [ ] Accessibility checks were completed.
- [ ] Recovery contracts were confirmed.

## 23. Technical Appendix A: Survey Flow

```
Embedded Data Initialization

Configuration Refresh - Level 1
Choose Solo/Group Mode
Team ID if Group Mode

Level 1
    Node 1.1
    Node 1.2
    Node 1.3
    Node 1.4
    Node 1.5
    Wednesday Decision Log
    Thursday Release Checkpoint

Configuration Refresh - Level 2
Thursday Recap
Curveball 1
Level 2
    Node 2.1
    Node 2.2
    Node 2.3
    Node 2.4
    Node 2.5
    Thursday Decision Log
    Friday Release Checkpoint

Configuration Refresh - Level 3
Friday Recap
Curveball 2
Node 3.1
Curveball 3
    Node 3.2
    Node 3.3
    Node 3.4
    Node 3.5
    Friday Decision Log
    Tuesday Release Checkpoint

Configuration Refresh - Level 4
Tuesday Recap
Level 4
    Node 4.1
    Node 4.2
    Node 4.3
    Node 4.4
    Node 4.5
    Final Recommendation
    Archetype Calculation
    Final Outcome
    Team Reflection

End of Survey
```

## 24. Technical Appendix B: File Schemas

**Scores source**

```
NodeKey
QID
ChoicePosition
ChoiceText
EnterpriseImpact
CriticalRigor
StrategicDecisiveness
StakeholderTrust
Resourcefulness
StateField
StateValue
Active
```

**Curveballs source**

```
CurveballID
PoolNumber
Category
PoolPosition
Title
Body
Active
```

**Archetypes source**

```
ArchetypeID
Title
Body
Active
```

**Settings source**
```
Setting
Value
Active
```

**Release schedule**
```
Level
UnlockAt
Timezone
Active
```

## 25. Technical Appendix C: Configuration URLs

|File/service|Current location| Version|
|---|---|---|
|scores.json|[Link to Scores JSON file](https://drive.google.com/file/d/15M-CGS0_3kpuEuDHXdMSlJi940-noeeA/view?usp=sharing)   |[Version]|
|content.json|[Link to Content JSON file](https://drive.google.com/file/d/1x7vSaClC0SwIUPMD5xYKID1vkK4Yd2th/view?usp=sharing)   |[Version]|
|Release schedule|[Link to Release Schedule Google sheet](https://docs.google.com/spreadsheets/d/1OtIUH017O9_c3Iu6NgrQPSEVaZbZBHqCsXRnLo81jzY/edit?usp=sharing)   |[Version]|
|Qualtrics survey|   |[Version]|

## 26. Technical Appendix D: Scores Conversion Script

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
  "StateValue"
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
    trimws(as.character(row$StateField))
  }

  state_value <- if (
    is.na(row$StateValue)
  ) {
    ""
  } else {
    trimws(as.character(row$StateValue))
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
    stateValue = state_value
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



