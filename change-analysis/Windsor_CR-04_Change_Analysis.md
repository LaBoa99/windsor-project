---
title: Change Analysis CR-04
subtitle: "New screen design and a virtual tour of each hall — how the change reuses the cloud of CR-02 and the storage of CR-03"
running_title: Change Analysis CR-04 — Windsor
author: General México Software Co.
date: 2026-11-06
lang: en
---

# The Change

**The client asks for a new design of the screens and for virtual reality, so that Clients can appreciate the environment of a hall before booking it.**

The change has two parts:

- **Redesign.** The Client's public screens get a new design, made by the Project Manager in Figma. The screens that CR-02 and CR-03 add are drawn once, already in the new design.
- **Virtual tour.** Each hall gets a 360° tour. The Client goes from the map to a city, to a center, and to a hall with its photos. A **Virtual tour** button opens a new screen with the tour.

The interview with the client fixed these points:

- **Content.** The company buys a 360° camera and the team captures the halls, so the images belong to the client.
- **Viewer.** Tours are shown with a free open-source web viewer, similar to Google Street View. The Client drags the image, or moves the phone, and sees the hall.
- **Devices.** The viewer works on modest phones and computers because it loads light image tiles first. A VR headset is optional: the same viewer switches to immersive mode if it detects one. If a device cannot show the tour, the button shows the normal photos.

Three assumptions complete the request:

- The redesign covers the Client's public screens only; staff and administration screens keep their design.
- Halls are captured empty. Different setups (banquet, auditorium) are left for a later change.
- The work goes after the MVP, in the same sprints as CR-02.

The project captures the six Monterrey halls. New venues capture their halls when they open, as an operating cost of the client. Two fixed options are left for the client:

- the same camera travels with an employee, with meals capped at $750 MXN per day, the deductible limit of the income tax law;
- the venue buys its own camera at the same price.

# Cost Analysis

The change costs $11,644 MXN once, in the first year: 0.97% of the $1,200,000 MXN budget. It has no recurring cost:

- the viewer is open source;
- the images live in the Object Storage that CR-03 already pays for.

## Recurring Cost

Table: Cost of the change {#tbl-recurring}

| Item | Detail | Monthly | Year 1 | Year 2 onward |
|---|---|---|---|---|
| 360° camera | Insta360 X5, standard package, official Insta360 Mexico store: $636.99 USD, bought once | — | $11,644 MXN | $0 MXN |
| 360° viewer | Open-source web viewer, no license | — | $0 MXN | $0 MXN |
| Storage and transfer | Inside the 1 TB of Object Storage paid by CR-03 | — | $0 MXN | $0 MXN |
| **Total of the change** | Before taxes | — | **$11,644 MXN** | **$0 MXN** |

## One-Time Effort

The change takes 3 person-weeks:

- 1 for the Project Manager: redesign of the Client screens in Figma;
- 1 for the Mid-level Developer: new screens;
- 1 for the Junior Developer: viewer integration and capture of the six Monterrey halls, with no travel.

At fixed salaries this time is worth $12,471 MXN, an internal cost of time.

## Position Against the Budget

The change is measured on the budget that CR-03 and CR-02 leave.

Table: Position of the change against the budget {#tbl-budget}

| Concept | Amount | Share of budget |
|---|---|---|
| Total committed in baseline v1, first year | $587,468 MXN | 49.0% |
| Committed by CR-03 and CR-02 | $75,356 MXN | 6.28% |
| Added by this change | $11,644 MXN | 0.97% |
| **Total committed, first year** | **$674,468 MXN** | **56.2%** |
| **Remaining against the $1,200,000 MXN budget** | **$525,532 MXN** | **43.8%** |

## Sensitivity

Table: Sensitivity of the cost of the change {#tbl-sensitivity}

| Scenario | Cost of the change | Change vs base |
|---|---|---|
| Base (price before taxes, about $18.28 MXN per USD) | $11,644 MXN | — |
| 16% IVA on the camera | $13,507 MXN | +$1,863 MXN |
| Peso weakens to $20 MXN per USD | $12,740 MXN | +$1,096 MXN |
| Both | $14,778 MXN | +$3,134 MXN |

# How Impact Is Measured

The impact is rated with the five dimensions of CR-01: Time, Architecture, Money, People, and Modules. Each dimension is Low, Medium, or High under the same conditions.

A section of the baseline takes the highest level among the dimensions it touches. The overall impact is the middle value of the five ratings.

# What Changes from v1 to v2

The D-nn numbers are proposed. The Configuration Manager confirms them when the CCB approves the change.

Table: Sections of the baseline changed from v1 to v2 {#tbl-changed}

| Section | v1 | v2 | Decisions | Impact |
|---|---|---|---|---|
| Project Scope;<br>Functional<br>Requirements | RF-SAL-004 stores hall photos. | Adds RF-CLI-007 "Virtual tour". RF-SAL-004 also stores 360° images. | D-25 | **Medium.** Modules: Client Management and Event Halls. |
| Non-Functional<br>Requirements | RNF-USAB-001 asks for a responsive interface. | Adds RNF-USAB-002 "Tour on modest devices": light tiles first, and normal photos when the device cannot show the tour. | D-25 | **Medium.** Architecture: a component in the Presentation layer. |
| Deliverables;<br>Agile Approach<br>and Schedule | Client screens designed per module. | A design system for the Client screens in Sprints 7–8. Capture and viewer in Sprints 9–10. Milestones unchanged. | D-26 | **Medium.** Time: absorbed together with CR-02. People: more design work for the Project Manager. |
| Third-Party<br>Software | — | A 360° camera ($11,644 MXN) and the capture policy for new venues. | D-27 | **Low.** Money: 0.97% of the budget. |

# Impact Assessment

Overall impact: **Medium**, the middle value of the five ratings in @tbl-impact (Low, Medium, Medium, Medium, Medium). The change is cheap because it lands on what CR-02 and CR-03 already build:

- the Client Service in the cloud;
- the Hall Administration, which uploads the images;
- the Object Storage, which keeps them.

Table: Impact of the change by dimension {#tbl-impact}

| Dimension | Impact | Basis |
|---|---|---|
| Time | Medium | 3 person-weeks absorbed in Sprints 7–10 with CR-02. Both milestones hold. |
| Architecture | Medium | Adds the viewer in the Presentation layer and 360° images in the Data layer. The deployment does not change. |
| Money | Low | $11,644 MXN once, 0.97% of the budget. |
| People | Medium | Same team. The redesign rests on the Project Manager, the only member with UI/UX design. |
| Modules | Medium | 2 of 7 modules change: Client Management and Event Halls. |

# Risk Measurement

The matrix of `artifacts/risk measurement.md` is applied as in CR-01. The assumptions of CR-01 are kept:

- 40 hours per person per week;
- 5,200 person-hours planned;
- 0 hours completed.

The budget is the $537,176 MXN that CR-03 and CR-02 leave.

Table: Risk measurement of CR-04 {#tbl-risk-cr04}

| Metric | Measurement | Result | Level | Source |
|---|---|---|---|---|
| Effort | 3 person-weeks × 40 h = 120 h ÷ 5,200 h | 2.31% | **Low** | This document, *One-Time Effort* |
| Budget | ($11,644 MXN in cash + $12,471 MXN of internal effort) ÷ $537,176 MXN remaining | 4.49% | **Low** | This document, @tbl-recurring and @tbl-budget |
| Time | 0 sprints added ÷ 13 sprints; both milestones hold | 0% | **No risk** | This document, @fig-schedule |
| Expertise | The least common skill is UI/UX design, which only the Project Manager has. The viewer is a library for Angular, which all three developers know | 1 member | **High** | Baseline v1, *Project Team and Roles* |
| Resource allocation | The camera is bought online with a delivery time that fits Sprints 9–10 | Delivery time fits the schedule | **Medium** | This document, @tbl-recurring |
| Legal compliance | The change is legal. The halls are captured empty, so no person appears in the images | Yes | **No risk** | This document, *The Change* |

Overall risk: **High**, driven by Expertise. It drops to Medium if the Mid-level Developer reviews the designs in Figma with the Project Manager, which also gives the Project Manager a backup.

# Tour Flow

@fig-tour shows how a tour is captured once and shown many times. Phones, computers, and headsets use the same viewer. @tbl-tour describes each step.

![Virtual tour flow in two parts. Capture and publication, once per hall: 1, the company's Insta360 X5 camera takes the 360° images; 2, the Hall Administration uploads them and cuts them into tiles of several resolutions; 3, Object Storage keeps them next to the hall photos; 4, the Client Service serves them to an open-source 360° viewer with no license. Display on the Client's device, only when the tour is opened: on a phone the Client drags or moves the phone and light tiles load first; on a computer the Client drags with the mouse and zooming loads detail; on an optional VR headset the same viewer switches to immersive mode; if the device has no 360° support, the normal photos are shown](../images/cr-04-tour-flow.svg "Capture and display of the virtual tour"){#fig-tour}

Table: Tour flow steps {#tbl-tour}

| Step | From → To | Message | Requirements |
|---|---|---|---|
| 1 | 360° camera → Hall Administration | 360° images of an empty hall | RF-SAL-004 |
| 2 | Hall Administration → Object Storage | Tiles of several resolutions | RF-SAL-004, RNF-USAB-002 |
| 3 | Client → Client Service | Opens the Virtual tour of a hall | RF-CLI-007 |
| 4 | Client Service → Client's device | Viewer and light tiles first; detail on zoom; photos if not supported | RF-CLI-007, RNF-USAB-001, RNF-USAB-002 |

# Schedule Impact

The change goes after the MVP, in the same sprints as CR-02, so the map and the tour are built already in the new design:

- the redesign in Sprints 7–8;
- the capture and the viewer in Sprints 9–10.

The MVP stays at week 14 and the final release at week 26.

![Timeline of the 26-week sprint schedule with the baseline v1 rows in gray and the rows of CR-04 in blue: the Client screen redesign in Figma in weeks 15 to 18, and the 360° capture of the six halls and the tour viewer in weeks 19 to 22; the MVP release stays at week 14 and the final release at week 26](../images/cr-04-schedule.svg "Sprint schedule; the blue rows carry the work added by this change"){#fig-schedule}

# Risks Added

Table: Risks added or extended by the change {#tbl-risks}

| Risk | Description | Impact |
|---|---|---|
| Design rework | If the redesign arrives after the screens of CR-02 are built, they are drawn twice. Both changes share Sprints 7–10 to avoid it. | Medium |
| Tour weight | 360° images are heavy. Without tiles, the tour would be slow on phones with a weak signal. | Medium |
| Outdated tours | A renovated hall keeps an old tour until someone captures it again. | Low |

<div style="break-before: page"></div>

# SWOT Analysis (FODA) of the Change

Table: SWOT of the change, centered on the people of the project {#tbl-swot}

| | Helpful | Harmful |
|---|---|---|
| Internal<br>(project team) | <strong>Strengths</strong><ul><li>The Project Manager already designs the screens in Figma.</li><li>The viewer is a library, and all three developers know Angular.</li><li>The capture needs no specialist: the camera stitches the 360° image itself.</li></ul> | <strong>Weaknesses</strong><ul><li>The redesign rests on the Project Manager, who also coordinates the rollout of CR-02.</li><li>Nobody else on the team designs screens.</li></ul> |
| External<br>(client, Clients) | <strong>Opportunities</strong><ul><li>Clients see a hall without traveling, which matters for venues in other cities.</li><li>The client owns the camera and the images, with no subscription.</li><li>New venues capture their own halls.</li></ul> | <strong>Threats</strong><ul><li>The client may want each hall shown with different setups, which multiplies the captures.</li><li>The client may expect full 3D, which this change does not include.</li></ul> |

# Alternatives Analyzed

Five ways of showing a hall were considered. Three were scored, and the company's own camera with an open-source viewer ranked first with 4.15 of 5.

Two were discarded before scoring:

- **A 3D model of each hall:** no member builds 3D models, and the risk would be Prohibitive.
- **Renting a capture service:** no vendor publishes a fixed price.

The weights are those of CR-01: schedule fit 40%, budget 20%, security 15%, availability 15%, and impact on the baseline 10%.

Table: Virtual tour alternatives evaluated {#tbl-alternatives}

| Alternative | Description | Annual cost | Effort (person-weeks) | Score |
|---|---|---|---|---|
| Own camera and open-source viewer (selected) | Insta360 X5; images in Object Storage; viewer on the Client Service | $11,644 MXN once | 3 | **4.15** |
| Hosted tour platform | Matterport Professional, 10 spaces, $90 USD per month billed yearly, embedded in an iframe, plus the camera | $31,386 MXN the first year | 2 | 4.05 |
| Google Street View | The camera's images published on Google Maps and shown with the free Embed iframe | $11,644 MXN once | 2.5 | 3.70 |

The hosted platform is faster to integrate but costs $19,742 MXN every year, and the tours disappear if the subscription stops.

Google Street View is free, but the images become public on Google Maps under Google's rules, and the client loses control of them.

# Change Request Form (F-01)

Table: Change request form of CR-04 {#tbl-f01}

| Section | Field | Value |
|---|---|---|
| Request | CR ID, title, date, requester | CR-04, "Screen redesign and virtual tour", 2026-11-06, client (venue owner) |
| Request | Original request and reason | "The client requests changes to the design of the screens; it implies the use of Virtual reality to appreciate the environment of a hall." |
| Analysis | Affected CIs | Baseline v1 (CI-DOC-02), requirements CSV (CI-REQ-01), schedule figure (CI-IMG-05), Client Service (CI-CODE-02); reuses Object Storage (CI-CFG-07) |
| Analysis | Risk per metric and overall | Effort Low, Budget Low, Time No risk, Expertise High, Resource allocation Medium, Legal No risk; overall **High** |
| Decision | Approved, rejected, or deferred | Pending CCB |
| Implementation | Branch, PRs, D-nn | `cr-04/redesign-virtual-tour`; proposed D-25 to D-27 |
| Verification | Date, verifier, result | Pending |
| Release | New versions | Baseline v2; release 2.0.0 |
| Closure | Date and archive | Pending |

State: **Analyzed**. Depends on CR-02 (Client Service, Hall Administration) and CR-03 (Object Storage).
