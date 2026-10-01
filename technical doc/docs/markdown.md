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

## Document Purpose

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

## Executive Summary

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
