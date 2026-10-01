**Content-Management Workflow**

### Editing scores
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

### Editing curveballs or archetypes
1. Edit the curveballs.csv, archetypes.csv, or settings.csv. 
2. Run the content-conversion script.
3. Generate a new content.json.
4. Upload it to the Qualtrics Files Library.
5. Update the file URL if necessary.
6. Test curveball and display endings.
7. Publish the survey.

### Editing the release schedule
1. Edit the release schedule Google Sheet. 
It will automatically update. 

### 18.4 Versioning convention
Recommended version format:

YYYY-MM-DD-HHMMSS

Record production versions here:
|Date|Survey version|Scores version|Content version|Editor|Summary|
|--|--|--|--|--|--|
|[Date] | [Version] | [Version] | [Version] | [Name] | [Change] | 
