# Scoring and Archetype Methodology

The scoring model describes the strategic behaviors demonstrated by participants’ choices. It is designed for reflection and debrief.

## Five-Dimension Rubric

Every choice is evaluated across the [following scoring dimensions](lxd/scoredimensions.md):

- Enterprise Impact
- Critical Rigor
- Strategic Decisiveness
- Stakeholder Trust
- Resourcefulness

Scores use a common behavioral scale:

| Score | Evidence demonstrated |
|---:|---|
| 0 | Not demonstrated |
| 1 | Limited |
| 2 | Developing |
| 3 | Strong |
| 4 | Exceptional |

Questions do not need to test every dimension equally. When a question provides no evidence for a dimension, all choices receive zero in that dimension. This avoids awarding artificial credit.

Choices should reflect meaningful tradeoffs. Administrators should review questions where one option dominates every dimension or all options produce nearly identical profiles.

## Dimension Normalization

Cumulative scores are normalized separately:

**Dimension percentage = earned points ÷ maximum available points across completed nodes × 100**

The maximum is the sum of the highest available choice score for that dimension at each completed node.

The five maxima may come from different, mutually incompatible choices. Therefore, percentages describe dimension-specific evidence—not progress toward one achievable “perfect strategy.”

## Archetype Classification

The model combines minimum qualification rules with [closest-profile matching](lxd/archetypes.md).

A profile qualifies when both defining dimensions reach the configured threshold.

Among qualifying approaches, the simulation selects the closest faculty-defined target profile using squared distance across all five normalized dimensions:

**Distance = sum of (participant dimension − target dimension)²**

If no approach qualifies, the simulation assigns **The Unaligned Agency**. This indicates that the configured qualification criteria were not met; it should not be treated as proof of poor teamwork or passive behavior without reviewing the decision logs.

Exact ties use a documented tie-breaker. 

## Midpoint and Final Use

The midpoint diagnostic uses decisions completed through that point. The final classification uses the completed engagement.

Both identify strategic tendencies and tradeoffs. A team’s final approach may differ from its midpoint profile.

## Faculty Calibration

Target profiles and qualification thresholds require faculty review. Before launch:

- Independently score choices using the rubric.
- Discuss rating differences of two or more points.
- Document the rationale for agreed scores.
- Test intended archetype paths, mixed profiles, and near-threshold cases.
- Confirm that outcomes support a credible instructional debrief.

Example target values are starting points, not validated measures of consulting performance.
