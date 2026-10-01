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
