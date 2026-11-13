---
title: Change Analysis CR-03
subtitle: "Anti-money-laundering reporting — how a law already in force reaches the booking flow before the MVP"
running_title: Change Analysis CR-03 — Windsor
author: General México Software Co.
date: 2026-11-06
lang: en
---

# The Change

**A new chapter of the anti-money-laundering law requires that every Client who rents a hall gives their personal data to the landlord, who reports it to the authorities for a background check. Compliance means acquiring a software module with cryptographic signing, connecting it to the authorities' server, and answering the authorities' requests for event data: number of attendees and nature of the event.**

The law is already in force. The system does not run in production yet, but the MVP (week 14) takes bookings, so it has to comply from the first day. A booking taken without the report would be illegal.

The report is a **transparency obligation, not an approval**. The booking does not wait for an answer from the authority.

The interview with the client fixed these points:

- **Who is reported.** The obligation applies to every Client, with no minimum amount, including Clients who book at the desk of a venue.
- **Data gate.** A booking is not confirmed until the identification data are complete. The report is sent afterwards, in the background, and its acknowledgment is kept as evidence.
- **Authority requests.** The authorities query the event data through an API that the system exposes to them.
- **Attendees.** The number of attendees comes from the digital invitations. The Client gives the number of guests and their data when sending the invitations (RF-INV-001). The QR scans at the door give the actual attendance.

This change also closes an open item of CR-01: the privacy notice required by the LFPDPPP. Personal data of Clients and now of guests are processed, so the notice and the guests' consent become actions of this change.

As a reference, the real anti-money-laundering law (LFPIORPI, article 17, section XV) already treats the rental of real estate as a vulnerable activity. After the July 2025 reform, records are kept for 10 years, and the minimum fine for not filing a report is about $1.17 million MXN.

# Cost Analysis

The change costs $63,948 MXN in cash in the first year and every year after: 5.33% of the $1,200,000 MXN budget. Its effort is absorbed by the existing team.

## Recurring Cost

Table: Recurring cost of the change {#tbl-recurring}

| Item | Detail | Monthly | Year 1 | Year 2 onward |
|---|---|---|---|---|
| AML module | Commercial anti-money-laundering software: generates and signs the reports in the authority's format and keeps the encrypted file. Published price of KYC Systems for property rental | $5,000 MXN | $60,000 MXN | $60,000 MXN |
| Object Storage | Vultr Object Storage, Standard tier: 1 TB of storage and 1 TB of transfer. Keeps the reports, acknowledgments, and authority queries for 10 years | $18 USD | $3,948 MXN | $3,948 MXN |
| **Total of the change** | Before taxes | — | **$63,948 MXN** | **$63,948 MXN** |

Object Storage is introduced by this change because the 10-year file is needed from the MVP onward. CR-02 and CR-04 reuse it for photos, 360° images, and site backups. Records older than a year can move to the archive tier at $0.006 USD per GB.

## One-Time Effort

The change takes 4 person-weeks:

- 2 for the Senior Developer: module integration, signing, and the authority API with mutual TLS;
- 1 for the Mid-level Developer: identification forms, data gate, and guest consent;
- 1 for the Junior Developer: desk capture at the venue and tests.

At fixed salaries this time is worth $17,552 MXN, an internal cost of time.

## Position Against the Budget

This change is implemented first, so it is measured on the budget left by baseline v1.

Table: Position of the change against the budget {#tbl-budget}

| Concept | Amount | Share of budget |
|---|---|---|
| Total committed in baseline v1, first year | $587,468 MXN | 49.0% |
| Added by this change | $63,948 MXN | 5.33% |
| **Total committed, first year** | **$651,416 MXN** | **54.3%** |
| **Remaining against the $1,200,000 MXN budget** | **$548,584 MXN** | **45.7%** |

## Sensitivity

Table: Sensitivity of the first-year cost of the change {#tbl-sensitivity}

| Scenario | Year-1 cost of the change | Change vs base |
|---|---|---|
| Base (prices before taxes, about $18.28 MXN per USD) | $63,948 MXN | — |
| 16% IVA on the module and Object Storage | $74,180 MXN | +$10,232 MXN |
| Peso weakens to $20 MXN per USD | $64,320 MXN | +$372 MXN |
| Both | $74,611 MXN | +$10,663 MXN |

The module is priced in pesos, so the exchange rate barely matters. IVA is the main sensitivity.

# How Impact Is Measured

The impact is rated with the five dimensions of CR-01: Time, Architecture, Money, People, and Modules. Each dimension is Low, Medium, or High under the same conditions.

A section of the baseline takes the highest level among the dimensions it touches. The overall impact is the middle value of the five ratings.

# What Changes from v1 to v2

The D-nn numbers are proposed. The Configuration Manager confirms them when the CCB approves the change.

Table: Sections of the baseline changed from v1 to v2 {#tbl-changed}

| Section | v1 | v2 | Decisions | Impact |
|---|---|---|---|---|
| Project Scope;<br>Functional<br>Requirements | 30 requirements; no identification of Clients for the authorities. | Adds RF-CLI-006 "Identification data", RF-COMP-005 "AML report", RF-COMP-006 "Authority query API", and RF-INV-006 "Guest data and consent". | D-21 | **High.** Modules: Client Management, Purchases and Payments, and Guests and QR change. |
| Non-Functional<br>Requirements | 6 requirements. | Adds RNF-SEG-003 "Record retention" (10 years, encrypted) and RNF-SEG-004 "Authority access" (mutual TLS, minimum data). | D-21 | **Medium.** Architecture: a new entry point through the existing Nginx. |
| Third-Party<br>Software | $9,968 MXN in the first year. | Adds the AML module ($60,000 MXN) and Object Storage ($3,948 MXN). | D-22 | **High.** Money: 5.33% of the budget. |
| Agile Approach<br>and Schedule | Sprints 3–4: Client Management and Suppliers; Sprints 5–6: Purchases and Guests. | Identification data and guest consent go in Sprints 3–4; the module, the report, and the authority API in Sprints 5–6. RF-PROV-004 "Supplier rating" moves to Sprints 7–8. | D-23 | **Medium.** Time: a sprint goal moves; both milestones hold. |
| Identified Risks;<br>Privacy | No privacy notice (pending action of CR-01). | Adds the LFPDPPP privacy notice and guest consent. Adds the risks of regulatory non-compliance and of exposure of identification data. | D-24 | **Medium.** People: the notice needs legal review outside the team. |

# Impact Assessment

Overall impact: **Medium**, the middle value of the five ratings in @tbl-impact (Medium, Medium, Medium, High, High).

The change is the most expensive of the three requests because of the module license. It touches three modules. The architecture keeps its shape: the module and the API live in the existing monolith until CR-02 moves them to the Client Service.

Table: Impact of the change by dimension {#tbl-impact}

| Dimension | Impact | Basis |
|---|---|---|
| Time | Medium | 4 person-weeks inside Sprints 3–6. RF-PROV-004 moves to Sprints 7–8 to make room. The MVP (week 14) and the final release (week 26) hold. |
| Architecture | Medium | Adds a component in the Integration layer (AML module), an API behind the existing Nginx, and Object Storage in the Data layer. Where the system runs does not change. |
| Money | High | $63,948 MXN in the first year, 5.33% of the budget. |
| People | Medium | Same team and salaries. Cryptographic signing and mutual TLS need a skill no role covers. |
| Modules | High | 3 of 7 modules gain requirements: Client Management, Purchases and Payments, and Guests and QR. |

# Risk Measurement

The matrix of `artifacts/risk measurement.md` is applied as in CR-01. The assumptions of CR-01 are kept:

- 40 hours per person per week;
- 5,200 person-hours planned;
- 0 hours completed.

The budget is the $612,532 MXN left by baseline v1.

Table: Risk measurement of CR-03 {#tbl-risk-cr03}

| Metric | Measurement | Result | Level | Source |
|---|---|---|---|---|
| Effort | 4 person-weeks × 40 h = 160 h ÷ 5,200 h | 3.08% | **Low** | This document, *One-Time Effort* |
| Budget | ($63,948 MXN in cash + $17,552 MXN of internal effort) ÷ $612,532 MXN remaining | 13.31% | **Medium** | This document, @tbl-recurring and @tbl-budget |
| Time | 0 sprints added ÷ 13 sprints. RF-PROV-004 moves inside the plan, and the MVP holds | 0% | **No risk** | This document, @fig-schedule |
| Expertise | The least common skill is cryptographic signing (e.firma) and mutual TLS with an external authority. No member lists it. The Senior Developer has related experience from the SSL of the v1 proxy | Partial coverage only | **Very high** | Baseline v1, *Project Team and Roles* |
| Resource allocation | Several vendors sell the module. Contracting it and registering the company with the authority take time, which fits if it starts in Sprint 3 | Delivery time fits the schedule | **Medium** | This document, @tbl-alternatives |
| Legal compliance | The change is what makes the system legal. Rejecting it would make every booking of the MVP illegal | Yes | **No risk** | Anti-money-laundering law, new chapter; LFPDPPP |

Overall risk: **Very high**, driven by Expertise. Mitigations:

- the vendor's onboarding training for the Senior Developer in Sprint 3;
- pairing with the Mid-level Developer on the signing and the API.

If the module is late, the **manual procedure** keeps the venue legal. The staff send the reports through the authority's portal until the module works. This is a contingency, not the solution: the law asks for the module.

# Architecture: Before and After

In the MVP, before CR-02, the module and the authority API live in the existing monolith on the Monterrey LAN. The authorities reach the API through the Nginx proxy on Vultr, as Clients do, but with mutual TLS. The reports and acknowledgments go to Object Storage.

When CR-02 is implemented, the module and the API move to the Client Service in the cloud (country view in CR-02, *Architecture: Before and After*). Venues with a weak link then never talk to the authority:

- A desk booking at a venue goes through the Sync Service.
- The Client Service sends the report.

# Reporting Flow

@fig-reporting shows both flows: the report sent for every booking, and the request the authority makes. @tbl-reporting describes each step.

![Reporting flow in two parts. Report, for every booking: 1, the Client gives identification data online, or the staff capture it at the desk, with consent recorded; 2, a data gate stops confirmation until the data are complete; 3, the site that owns the hall confirms the booking; 4, the AML module signs and sends the report and retries if it fails; 5, the acknowledgment is stored in Object Storage for 10 years. The booking does not wait for the authority. Request, when the authority asks: A, the authorities' server sends a query signed with its certificate; B, Nginx and the authority API accept it over mutual TLS and answer only what the law asks; C, the event data come from the invitations and the nature of the event; D, every request and answer is logged in Object Storage. Actual attendance comes from the QR scans at the door, sent by each site through the Sync Service](../images/cr-03-reporting-flow.svg "Report and request flows"){#fig-reporting}

Table: Reporting flow steps {#tbl-reporting}

| Step | From → To | Message | Requirements |
|---|---|---|---|
| 1 | Client or staff → Client Management | Identification data and consent | RF-CLI-006, RF-INV-006 |
| 2 | Client Management → Event Halls | Booking request, only with complete data | RF-CLI-006, RF-SAL-003 |
| 3 | Event Halls → Purchases and Payments | Booking confirmed without overlap | RF-SAL-003, RF-COMP-002 |
| 4 | Purchases and Payments → AML module → authorities | Signed report, sent in the background with retries | RF-COMP-005 |
| 5 | AML module → Object Storage | Acknowledgment kept for 10 years | RNF-SEG-003 |
| A–D | Authorities → Nginx → API → Object Storage | Query and answer about attendees and nature of the event, logged | RF-COMP-006, RNF-SEG-004 |

# Schedule Impact

The change goes before the MVP because the law is already in force:

- identification data and consent in Sprints 3–4, together with the booking view;
- the module, the report, and the authority API in Sprints 5–6, together with Purchases and Payments and Guests and QR.

To make room, RF-PROV-004 "Supplier rating" moves to Sprints 7–8. A Client can book without rating a supplier, so it is the least critical story in those sprints. The MVP stays at week 14 and the final release at week 26.

![Timeline of the 26-week sprint schedule with the baseline v1 rows in gray and the rows of CR-03 in blue: identification data and guest consent in weeks 7 to 10, and the AML module, report, and authority API in weeks 11 to 14, before the MVP release at week 14; a dashed row shows Supplier rating, RF-PROV-004, moved to weeks 15 to 18; the final release stays at week 26](../images/cr-03-schedule.svg "Sprint schedule; the blue rows carry the work added by this change"){#fig-schedule}

# Risks Added

Table: Risks added or extended by the change {#tbl-risks}

| Risk | Description | Impact |
|---|---|---|
| Regulatory non-compliance | If a report is not sent or its acknowledgment is lost, the client faces fines of over $1 million MXN. The retries and the stored acknowledgments reduce the risk. | High |
| Exposure of identification data | The system now keeps identity data of every Client and data of guests for 10 years. A leak would breach the LFPDPPP. | Med-high |
| Third-party dependency<br>(extended) | Adds the AML vendor and Vultr Object Storage. A change in the vendor's format or price affects every booking. | Medium |
| Authority entry point | The authority API is a new way into the system from the internet, protected by mutual TLS. | Medium |

<div style="break-before: page"></div>

# SWOT Analysis (FODA) of the Change

Table: SWOT of the change, centered on the people of the project {#tbl-swot}

| | Helpful | Harmful |
|---|---|---|
| Internal<br>(project team) | <strong>Strengths</strong><ul><li>The module is bought, so the team integrates it instead of writing cryptography.</li><li>The guest data reuse the invitation flow of Guests and QR, which the team already builds.</li><li>The Project Manager designs the forms and the consent screens in Figma.</li></ul> | <strong>Weaknesses</strong><ul><li>No member knows cryptographic signing or mutual TLS. The work falls on the Senior Developer.</li><li>The team has no legal role, and the privacy notice needs outside review.</li><li>Sprints 3–6 were already the busiest of the plan.</li></ul> |
| External<br>(client, Clients,<br>authorities) | <strong>Opportunities</strong><ul><li>The client complies with the law from the first booking and avoids the fines.</li><li>The privacy notice pending since CR-01 is finally published.</li><li>Object Storage, needed here, serves CR-02 and CR-04 at no extra cost.</li></ul> | <strong>Threats</strong><ul><li>Some Clients may give up when asked for identity data.</li><li>The authority can change its format, forcing a vendor update.</li><li>Guests may refuse to give their data in the invitation.</li></ul> |

# Alternatives Analyzed

Six ways of complying were considered. Three were scored, and the commercial module in the monolith with records in Object Storage ranked first with 3.90 of 5.

Three were discarded before scoring:

- **Manual procedure only:** the law requires the module. The manual procedure is kept as a contingency.
- **Developing the cryptographic module in house:** no member has the skill, and it would not be certified by the authority.
- **The vendor's web portal with data typed twice:** the system could not answer the authority's requests from its own data.

The weights are those of CR-01: schedule fit 40%, budget 20%, security 15%, availability 15%, and impact on the baseline 10%.

Table: Compliance alternatives evaluated {#tbl-alternatives}

| Alternative | Description | Annual cost | Effort (person-weeks) | Score |
|---|---|---|---|---|
| Module in the monolith, records in Object Storage (selected) | Commercial module and authority API in the current monolith; reports and acknowledgments kept off site and encrypted; moves to the Client Service with CR-02 | $63,948 MXN | 4 | **3.90** |
| Module in the monolith, records on the LAN | Same, but the 10-year file stays on the venue's server | $60,000 MXN | 3.5 | 3.55 |
| Module as a separate cloud service | Brings part of the CR-02 cloud forward to before the MVP | $73,382 MXN | 6 | 2.95 |

Keeping the records on the LAN saves $3,948 MXN per year. The file would then depend on one server at one venue for 10 years, and the authority could not query it during a link outage.

# Change Request Form (F-01)

Table: Change request form of CR-03 {#tbl-f01}

| Section | Field | Value |
|---|---|---|
| Request | CR ID, title, date, requester | CR-03, "Anti-money-laundering reporting", 2026-11-06, client (venue owner) |
| Request | Original request and reason | "The Mexican government issued a new chapter of its anti-laundering law that requires that each client who rents a hall should provide his/her personal data to the landlord for a background check by the authorities. Complying with the requirement means acquiring a software module with sophisticated cryptographic algorithms and connecting it to the authorities' server. The authorities also request additional information about the event, such as the number of attendees and the nature of the event." |
| Analysis | Affected CIs | Baseline v1 (CI-DOC-02), requirements CSV (CI-REQ-01), schedule figure (CI-IMG-05), monolith (CI-CODE-01), Nginx configuration (CI-CFG-01); new CI-CFG-06 (AML module) and CI-CFG-07 (Object Storage) |
| Analysis | Risk per metric and overall | Effort Low, Budget Medium, Time No risk, Expertise Very high, Resource allocation Medium, Legal No risk; overall **Very high** |
| Decision | Approved, rejected, or deferred | Pending CCB. Rejecting it is not a real option: the law is in force |
| Implementation | Branch, PRs, D-nn | `cr-03/aml-reporting`; proposed D-21 to D-24 |
| Verification | Date, verifier, result | Pending |
| Release | New versions | Baseline v2; release 1.0.0 (MVP) |
| Closure | Date and archive | Pending |

State: **Analyzed**. Implemented first; CR-02 and CR-04 reuse its Object Storage.
