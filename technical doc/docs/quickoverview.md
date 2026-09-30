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
