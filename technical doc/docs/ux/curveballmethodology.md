# Curveballs

Participants encounter a shared passenger rail announcement at the beginning of Level 2 and an assigned business development later in the simulation.

Each event appears as a newsflash containing a title and description, followed by decisions about its implications.

## Group and Sandbox Modes

**Group mode:** The Team ID determines the assigned event through a repeating 16-record sequence. Teams 1–16 receive records 1–16; Team 17 receives record 1 again. Different teams may therefore receive the same event.

**Sandbox mode:** Each new playthrough receives a random assignment. Refreshing or resuming the same response should preserve that assignment.

## Display and Progression

Teams review the newsflash and discuss how it affects their current recommendation before continuing.

Titles and descriptions are loaded from `content.json`, allowing administrators to update narrative content without editing individual Qualtrics questions.

Because group assignment depends on array order, administrators must keep the curveball order in `content.json` stable after launch. Reordering records changes which event a Team ID receives.

Production mode hides technical IDs and future assignments. Development mode may display assignment details for testing.
