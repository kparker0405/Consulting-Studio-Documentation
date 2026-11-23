# Strategic Profiles and Archetypes

Participants receive feedback about how their choices shaped their strategy. The feedback includes a five-dimensional profile and, at designated points, an archetype narrative.

## Strategic Profile Display

The profile presents five labeled continuums:

- Enterprise Impact
- Critical Rigor
- Strategic Decisiveness
- Stakeholder Trust
- Resourcefulness

Each continuum includes:

- A short dimension description
- A percentage from 0–100
- A visual meter
- Earned points and maximum available points

The percentage is calculated as:

**Earned points ÷ maximum available points across completed nodes × 100**

The display explains that these results describe decision patterns—not a grade or a single correct strategy. Lower scores may reflect tradeoffs in favor of other priorities.

## Midpoint and Final Feedback

| Placement | Feedback |
|---|---|
| Before Level 3 | Provisional archetype diagnostic highlighting the emerging approach and a tradeoff to monitor |
| After the final recommendation | Strategic profile and final archetype debrief connecting the team's decisions to implementation priorities |

The final archetype may differ from the midpoint classification as participants make additional decisions.

## Content and Technical Behavior

Profile percentages are calculated from cumulative response scores, the completed-node ledger, and the scoring configuration.

Archetype titles and narratives are maintained in the source spreadsheet and loaded from `content.json`. Midpoint and final profile IDs should be stored separately.

The display provides text equivalents and accessible meter labels. If scores or configuration mappings are invalid, it shows an error rather than presenting misleading results.

## Production and Development Modes

The strategic profile percentages and point totals are intentional participant-facing feedback.

Development mode may additionally show technical identifiers, configuration versions, classification calculations, and troubleshooting details. Those technical diagnostics remain hidden in production.
