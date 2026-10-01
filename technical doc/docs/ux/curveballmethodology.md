**Curveball Methodology**
### Curveball pools
|Pool|Category|Number of events| Placement|
|----|------|-----|----|
|1|Tourism and Regional Infrastructure|5|Before Level 2|
|2| Economic and Corporate Landscape|5|Before Node 3.1|
|3A|Local Policy and Community Relations|5|After Node 3.1|
|3B|Competitive Environment and Market Dynamics|5|After Node 3.1|

### Group assignment
Group-mode curveballs are assigned deterministcally using the Team ID. This provides:
- Consistency for all members of the same team
- Balanced assignment across up to 100 groups
- Reproducible assignments
- Comparable but nonidentical team experiences

The formula that designates which curveball is assigned to which group is based off of calculates done to to their team number. 

For Pool 1, each of the five events appears 20 times across Teams 1-100.

For Pool 2, we use a different balanced pattern so its assignment is not identifcal to pool 1, and each of the five events appears 20 times across Teams 1-100.

For Pool 3, odd-numbered teams are designated a curveball from the Community/Policy category, and even-numbered teams is designated a curveball from the Competition/Market category. Each event in the selected Pool 3 category appears 10 times across Teams 1-100. 

### Solo assignment
Solo participants receive:
- One random event from Pool 1
- One random event from Pool 2
- One random Pool 3 category
- One random event from the selected Pool 3 category

The selected events are stored in embedded variables and remain fixed for the duration of the simulation. 

### Curveball content
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
