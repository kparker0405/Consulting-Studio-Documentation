# Beyond the Grandstand: Qualtrics Simulation Methodology and Technical Reference

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