# Content-Management Workflow

Administrators maintain simulation content and scoring in spreadsheets, export the source CSV files, and convert them into JSON files hosted in the Qualtrics Files Library.

The release schedule remains in a separate Google Sheet and is retrieved through Apps Script.

## Source Files

| CSV file | Purpose |
|---|---|
| `scores.csv` | Choice-level scores across the five strategic dimensions and optional strategic-state changes |
| `archetypes.csv` | Archetype titles, midpoint diagnostics, final debriefs, and classification parameters included in the configured schema |
| `curveballs.csv` | Event IDs, assignment order, categories, titles, and descriptions |
| `nodetitle_body.csv` | Decision-node titles and introductory/body text |
| `settings.csv` | Development-mode setting and other configured simulation controls |

The conversion scripts determine which source files are included in each JSON output. Keep those mappings documented alongside the scripts.

## Editing Scores

1. Open the scoring source spreadsheet.
2. Update the relevant rows in `scores.csv`.
3. Confirm that each scored question has five choices and that scores use the **0–4 rubric**.
4. Export the file as **CSV UTF-8**.
5. Run the [scoring-conversion script][appendix/scorescript.md] to generate `scores.json`.
6. Upload the new JSON file to the Qualtrics Files Library.
7. Update the configuration-loader URL if the file URL changed.
8. Begin a fresh test response and verify the configuration version.
9. Test score accumulation, dimension percentages, and archetype classification.
10. Publish any changed survey code or file URLs.

## Editing Narrative Content and Settings

Use this workflow for:

- `archetypes.csv`
- `curveballs.csv`
- `nodetitle_body.csv`
- `settings.csv`

1. Edit the relevant source spreadsheet.
2. Export the updated files as **CSV UTF-8**.
3. Run the [content-conversion script](appendix/contentscript.md) to generate `content.json`.
4. Upload the new JSON file to the Qualtrics Files Library.
5. Update configuration-loader URLs if the file URL changed.
6. Begin a fresh test response and verify the content version.
7. Test the affected displays: node text, curveballs, midpoint diagnostics, or final debriefs.
8. Confirm that development mode is disabled for student use.
9. Publish any changed survey code or file URLs.

**Assignment stability:** Curveball assignment uses array order. Keep curveball IDs and record order stable after launch so the same Team ID continues to receive the same event.

Updating CSV files alone does not update the simulation. The JSON must be regenerated, uploaded, and referenced by the survey.

## Editing the Release Schedule

1. Open the [Release Schedule Google Sheet](appendix/configurls.md).
2. Update the appropriate `Level`, `UnlockAt`, `Timezone`, or `Active` value.
3. Use the required date format: `YYYY-MM-DD HH:MM:SS`.
4. Test the corresponding release endpoint and Qualtrics checkpoint.

Sheet-value changes do not normally require Apps Script redeployment or survey publication. They take effect when the checkpoint next requests the schedule.

If the Apps Script code changes, deploy a new script version. If the endpoint URL changes, update the Qualtrics checkpoint JavaScript and publish the survey.

## Versioning and Change Records

Use a version identifier such as:

`YYYY-MM-DD-HHMMSS`

Record:

- Scores JSON version
- Content JSON version
- Qualtrics published version
- Date and editor
- Summary of changes
- Testing completed
- Any effect on active responses

Keep an archived copy of each production CSV and JSON set.

### Production Version Log

<iframe
  src="https://docs.google.com/spreadsheets/d/e/2PACX-1vSUcIfEVA9lHY0E7GY_rVvJm8C5OTiR4kJh0v_psnDEB1bbk5yY8SyNPiv3BqQsuoKoEoRzf1eflnT8/pubhtml?gid=0&amp;single=true&amp;widget=true&amp;headers=false"
  title="Simulation production version log"
  style="width:100%; height:450px; border:1px solid #cccccc;"
  loading="lazy"
></iframe>

[Open the version log](https://docs.google.com/spreadsheets/d/1r2S3P2DchtPmd6LSUihmRT2M1pZ7Ex3kiOmi-fh-n_k/edit?usp=sharing)

Editing requires permission to the spreadsheet. Because the embedded version log is published, do not include credentials, student records, or other restricted information in it.
