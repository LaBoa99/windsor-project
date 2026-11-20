---
title: Consolidated Change Analysis CR-02 to CR-04
subtitle: "Anti-money-laundering reporting, venues across the country, and a virtual tour — the combined effect of three change requests on baseline v1"
running_title: Consolidated Change Analysis CR-02 to CR-04 — Windsor
author: General México Software Co.
date: 2026-11-13
lang: en
---

[TOC]

# Purpose

This document summarizes the three change requests the client submitted on 2026-11-06 and measures their combined effect on baseline v1. Each change has its own analysis (CR-02, CR-03, and CR-04), which holds the detail: architecture, flows, SWOT, and the F-01 form. This document adds what no single analysis shows:

- the order in which the changes are implemented, and why;
- the total cost and effort against the budget;
- the combined impact and risk;
- the configuration items that more than one change touches.

The SCMP asks the CCB to review pending change requests together, not one by one (*Coordinating Multiple Changes*). This document is the input for that review.

# The Three Changes

Table: Summary of the change requests {#tbl-summary}

| CR | Request | What the change does | Release | Overall impact | Overall risk |
|---|---|---|---|---|---|
| CR-03 | Anti-money-laundering reporting | Asks every Client for identification data, signs and sends a report to the authorities with a purchased module, exposes an API for their queries, and keeps the records for 10 years | 1.0.0 (MVP, week 14) | Medium | Very high |
| CR-02 | Venues across the country and location map | Splits the system into a Site node per venue plus a Client Service and a Hall Administration in the cloud, joined by a Sync Service; adds a two-level map of the venues | 2.0.0 (week 26) | Medium | Very high |
| CR-04 | Screen redesign and virtual tour | Redesigns the Client's public screens and adds a 360° tour of each hall, captured with the company's own camera | 2.0.0 (week 26) | Medium | High |

All three are in state **Analyzed** and wait for the CCB. Budget ($1,200,000 MXN), timeline (6 months), milestones, and team stay as in baseline v1.

# Implementation Order and Dependencies

The changes are implemented in the order CR-03, CR-02, CR-04:

1. **CR-03 first.** The law is already in force, and the MVP takes bookings, so the system has to comply from the first day. It is the only change that goes before the MVP. It introduces Object Storage, which the other two reuse.
2. **CR-02 second.** It creates the Client Service and the Hall Administration in the cloud. When it is implemented, the AML module and the authority API of CR-03 move from the monolith to the Client Service.
3. **CR-04 third, in the same sprints as CR-02.** The tour runs on the Client Service and the Hall Administration of CR-02 and stores its images in the Object Storage of CR-03. Sharing the sprints with CR-02 means the map screens are drawn once, already in the new design.

Each analysis measures its change against the budget that the previous ones leave, following the SCMP rule for changes that touch the same items.

# Combined Cost

Table: Cost of the three changes {#tbl-cost}

| CR | Cash, year 1 | Cash, year 2 onward | Effort (person-weeks) | Value of the effort | Main item |
|---|---|---|---|---|---|
| CR-03 | $63,948 MXN | $63,948 MXN | 4 | $17,552 MXN | AML module, $60,000 MXN per year |
| CR-02 | $11,408 MXN | $11,408 MXN | 8 | $37,414 MXN | Two Vultr VPS, net of the v1 proxy |
| CR-04 | $11,644 MXN | $0 MXN | 3 | $12,471 MXN | 360° camera, bought once |
| **Total** | **$87,000 MXN** | **$75,356 MXN** | **15** | **$67,437 MXN** | — |

The effort is absorbed by the existing team at fixed salaries, so it is an internal cost of time, not a new expense. The hardware of each new venue is bought by the client and is outside the project budget (CR-02, *Minimum hardware of a new venue*).

## Position Against the Budget

Table: Position of the three changes against the budget {#tbl-budget}

| Concept | Amount | Share of budget |
|---|---|---|
| Total committed in baseline v1, first year | $587,468 MXN | 49.0% |
| Added by CR-03 | $63,948 MXN | 5.33% |
| Added by CR-02 | $11,408 MXN | 0.95% |
| Added by CR-04 | $11,644 MXN | 0.97% |
| **Total committed, first year** | **$674,468 MXN** | **56.2%** |
| **Remaining against the $1,200,000 MXN budget** | **$525,532 MXN** | **43.8%** |

The three changes together take 7.25% of the budget. CR-03 alone is three quarters of that, because of the module license.

## Sensitivity

With 16% IVA and the peso at $20 MXN per USD, the worst case of each analysis adds up to $103,868 MXN in the first year: 8.66% of the budget. The project keeps $508,664 MXN unspent even then. IVA is the main sensitivity, because the largest item, the AML module, is priced in pesos.

# Effort per Person

Table: Effort of the three changes per team member, in person-weeks {#tbl-effort}

| Member | CR-03 | CR-02 | CR-04 | Total | What falls on them |
|---|---|---|---|---|---|
| Senior Developer | 2 | 4 | — | 6 | Module signing, authority API with mutual TLS, Sync Service, tunnels, and rollout |
| Mid-level Developer | 1 | 2 | 1 | 4 | Identification forms and consent, the three-part split, center registry, new screens |
| Junior Developer | 1 | 1 | 1 | 3 | Desk capture, the map, viewer integration, and capture of the six halls |
| Project Manager | — | 1 | 1 | 2 | Site hardware and rollout coordination, redesign in Figma |
| **Total** | **4** | **8** | **3** | **15** | — |

The Senior Developer carries 40% of the added effort and every skill no member lists: cryptographic signing, mutual TLS, data sync, and multi-site VPN routing. This is the main finding of this document.

# Combined Schedule

No sprint is added. Both milestones hold: the MVP at week 14 and the final release at week 26.

Table: Work added to each sprint block {#tbl-schedule}

| Sprints | Weeks | Work added | CR |
|---|---|---|---|
| 3–4 | 7–10 | Identification data and guest consent, with the booking view | CR-03 |
| 5–6 | 11–14 | AML module, report, and authority API, before the MVP | CR-03 |
| 7–8 | 15–18 | Three-part split and Sync Service | CR-02 |
| 7–8 | 15–18 | Redesign of the Client screens in Figma | CR-04 |
| 7–8 | 15–18 | RF-PROV-004 "Supplier rating", moved from Sprints 3–4 | CR-03 |
| 9–10 | 19–22 | Map, center registry, and site rollout kit | CR-02 |
| 9–10 | 19–22 | 360° capture of the six halls and the tour viewer | CR-04 |
| 11–12 | 23–26 | Hardening, as in baseline v1 | — |

Sprints 7–10 carry two changes on top of their planned work. Hardening keeps weeks 19–26, but shares weeks 19–22 with CR-02 and CR-04, so the room for fixing defects before the final release shrinks.

# What Changes from v1 to v2

The three changes go into one baseline version, v2, as D-01 to D-13 went into v1. The D-nn numbers are proposed; the Configuration Manager confirms them when the CCB approves the changes.

Table: Decisions of the three changes, by section of the baseline {#tbl-changed}

| Section | CR-03 | CR-02 | CR-04 |
|---|---|---|---|
| General Information;<br>Justification and Objective | — | D-14: several venues, each with its own LAN | — |
| Project Scope;<br>Functional Requirements | D-21: 4 requirements | D-15: 3 requirements; roles per venue | D-25: 1 requirement; 360° images |
| Non-Functional Requirements | D-21: retention and authority access | D-17: site autonomy; RNF-SEG-002 relaxed | D-25: tour on modest devices |
| Proposed Technologies;<br>Architecture | — | D-16: three parts and a Sync Service | — |
| Third-Party Software | D-22: AML module and Object Storage | D-18: Vultr, Docker Hub, Maps Embed | D-27: 360° camera |
| Deliverables;<br>Agile Approach and Schedule | D-23: Sprints 3–6 | D-19: Sprints 7–10 | D-26: design system, Sprints 7–10 |
| Identified Risks; Privacy | D-24: privacy notice and consent | D-20: sync and client hardware | — |

## Requirements Added

Table: Requirements added or modified by the three changes {#tbl-requirements}

| CR | New functional | New non-functional | Modified |
|---|---|---|---|
| CR-03 | RF-CLI-006, RF-COMP-005, RF-COMP-006, RF-INV-006 | RNF-SEG-003, RNF-SEG-004 | — |
| CR-02 | RF-SAL-006, RF-SAL-007, RF-CLI-005 | RNF-DISP-002 | RF-AUTH-003, RNF-SEG-002 |
| CR-04 | RF-CLI-007 | RNF-USAB-002 | RF-SAL-004 |
| **Total** | **8** | **4** | **3** |

The SRS of baseline v2 grows from 30 to 38 functional and from 6 to 10 non-functional requirements. The requirements list (CI-REQ-01) has to be updated in the same edit. It already differs from baseline v1 (Configuration Item Register, *Requirements*), so that difference has to be corrected before baseline v2 is published.

# Combined Impact

The impact uses the five dimensions of CR-01. For each dimension, the combined rating is the highest of the three changes. The overall impact is the middle value of the five combined ratings.

Table: Impact of the three changes by dimension {#tbl-impact}

| Dimension | CR-03 | CR-02 | CR-04 | Combined | Basis |
|---|---|---|---|---|---|
| Time | Medium | Medium | Medium | **Medium** | 15 person-weeks absorbed in Sprints 3–10; both milestones hold |
| Architecture | Medium | High | Medium | **High** | The monolith becomes three parts plus a Sync Service, with an AML module, an authority API, Object Storage, and a 360° viewer |
| Money | High | Low | Low | **High** | $87,000 MXN in the first year, 7.25% of the budget |
| People | Medium | Medium | Medium | **Medium** | Same team and salaries; the new skills fall on the Senior Developer, and the design on the Project Manager |
| Modules | High | High | Medium | **High** | 5 of 7 modules change: Authentication, Event Halls, Client Management, Purchases and Payments, and Guests and QR |

Combined overall impact: **High** (Medium, Medium, High, High, High). Each change alone is Medium; together they reshape the architecture and touch five of the seven modules.

# Combined Risk Measurement

The risk metrics matrix (CI-DOC-05) is applied to the three changes as one, with the assumptions of CR-01:

- 40 hours per person per week;
- 5,200 person-hours planned;
- 0 hours completed.

The budget is the $612,532 MXN left by baseline v1.

Table: Risk measurement of the three changes {#tbl-risk}

| Metric | CR-03 | CR-02 | CR-04 | Combined measurement | Combined level |
|---|---|---|---|---|---|
| Effort | Low | Medium | Low | 15 person-weeks × 40 h = 600 h ÷ 5,200 h = 11.54% | **Medium** |
| Budget | Medium | Low | Low | ($87,000 MXN in cash + $67,437 MXN of internal effort) ÷ $612,532 MXN = 25.21% | **High** |
| Time | No risk | No risk | No risk | 0 sprints added; both milestones hold | **No risk** |
| Expertise | Very high | Very high | High | Signing, mutual TLS, data sync, and VPN routing: no member lists them; the Senior Developer has related experience | **Very high** |
| Resource allocation | Medium | Medium | Medium | Module, VPS, camera, and client hardware: all with delivery times that fit the schedule | **Medium** |
| Legal compliance | No risk | No risk | No risk | All three are legal; CR-03 is what makes the MVP legal | **No risk** |

Overall risk: **Very high**, driven by Expertise, as in each change alone.

Budget is the metric that grows when the changes are added up. Each change alone is Low or Medium, but together they take a quarter of the remaining budget and reach **High**. The cash is small; most of it is the AML license and the value of the team's time.

The mitigations of each analysis lower the overall risk:

- vendor onboarding for the Senior Developer in Sprint 3 (CR-03);
- training on WireGuard with several peers and on the sync design before Sprint 7 (CR-02);
- pairing the Mid-level Developer with the Senior Developer on the signing, the API, and the Sync Service, which also gives the Senior Developer a backup (CR-03, CR-02);
- the Mid-level Developer reviewing the designs in Figma with the Project Manager (CR-04).

With all four in place, Expertise drops to Medium and the overall risk follows the Budget metric to **High**.

# Risks Added

Table: Risks added or extended by the three changes {#tbl-risks}

| Risk | CR | Impact |
|---|---|---|
| Regulatory non-compliance: a report not sent or an acknowledgment lost means fines of over $1 million MXN | CR-03 | High |
| Exposure of identification data kept for 10 years | CR-03 | Med-high |
| Third-party dependency (extended): AML vendor and Vultr Object Storage | CR-03 | Medium |
| Authority entry point: a new way into the system from the internet | CR-03 | Medium |
| Data sync between sites: the map may show dates that are gone | CR-02 | Medium |
| Client hardware: a late or undersized server delays a venue | CR-02 | Medium |
| Hall internet link (extended) to every venue | CR-02 | Medium |
| Version drift between venues | CR-02 | Medium |
| Design rework if the redesign arrives after the screens of CR-02 | CR-04 | Medium |
| Tour weight on phones with a weak signal | CR-04 | Medium |
| Outdated tours after a renovation | CR-04 | Low |

None of the three adds a people risk, but all of them make the key-person dependency (High) heavier. If the Senior Developer is reassigned during Sprints 3–10, the three changes stop together.

# Alternatives Selected

Table: Alternative selected in each change {#tbl-alternatives}

| CR | Alternatives considered | Selected | Score | Runner-up |
|---|---|---|---|---|
| CR-03 | 6 (3 scored) | Commercial module in the monolith, records in Object Storage | 3.90 | Records on the LAN, 3.55 |
| CR-02 | 8 (4 scored) | Three-part split with availability sync | 3.45 | VPS proxy per venue, 3.35 |
| CR-04 | 5 (3 scored) | Own 360° camera and open-source viewer | 4.15 | Hosted tour platform, 4.05 |

The three selections fit together: the Object Storage chosen in CR-03 is what makes CR-02's site backups and CR-04's tours free.

# Configuration Items

Each analysis lists the items it creates and modifies (section *Configuration Items*). Together the three changes create 25 items and modify 10 items that existed before them. @tbl-shared-ci lists the items that more than one change touches. The SCMP asks the CCB to set the order of changes that affect the same item; here the order is the one of *Implementation Order and Dependencies*.

Table: Configuration items touched by more than one change {#tbl-shared-ci}

| ID | Name | CR-03 | CR-02 | CR-04 |
|---|---|---|---|---|
| CI-DOC-02 | Baseline v1 | Modifies | Modifies | Modifies |
| CI-DOC-12 | Baseline v2 | Creates | Creates | Creates |
| CI-REQ-01 | Requirements list | Modifies | Modifies | Modifies |
| CI-IMG-04 | Module communication (Figure 4) | Modifies | Modifies | — |
| CI-IMG-05 | Sprint schedule (Figure 1) | Modifies | Modifies | Modifies |
| CI-CODE-01 | Windsor monolith | Modifies (1.0.0) | Splits (2.0.0) | — |
| CI-CODE-02 | Client Service image | Receives the module (2.0.0) | Creates | Modifies |
| CI-CODE-03 | Hall Administration image | — | Creates | Modifies |
| CI-DB-01 | Site database schema | Modifies (1.0.0) | Modifies (2.0.0) | — |
| CI-CFG-01 | Nginx proxy configuration | Modifies (1.0.0) | Modifies (2.0.0) | — |
| CI-CFG-07 | Object Storage bucket and retention rules | Creates | Modifies | Modifies |

The location, version, and hash of every item are in the Configuration Item Register.

# Recommendation to the CCB

Approve the three changes, in this order and with these conditions:

1. **Approve CR-03 now**, so the work starts in Sprint 3. The law is in force and rejecting it would make every booking of the MVP illegal. Contract the AML module and register the company with the authority at the start of Sprint 3; keep the manual procedure ready as a contingency.
2. **Approve CR-02 and CR-04 together** for release 2.0.0. They share Sprints 7–10 and the new screens, so approving one without the other means drawing the screens twice.
3. **Condition the approval on the mitigations** of *Combined Risk Measurement*, starting with the pairing on the Senior Developer's work.
4. **Correct the requirements list** (CI-REQ-01) through its own change request before baseline v2 is published.

Table: State of the change requests {#tbl-state}

| CR | State | Proposed decisions | Branch | Release |
|---|---|---|---|---|
| CR-03 | Analyzed | D-21 to D-24 | `cr-03/aml-reporting` | 1.0.0 |
| CR-02 | Analyzed | D-14 to D-20 | `cr-02/multi-site-map` | 2.0.0 |
| CR-04 | Analyzed | D-25 to D-27 | `cr-04/redesign-virtual-tour` | 2.0.0 |
