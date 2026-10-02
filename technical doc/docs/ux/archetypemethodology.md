## 12. Final Archetype Methodology

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
