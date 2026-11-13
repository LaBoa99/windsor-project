---
title: Change Analysis CR-02
subtitle: "Venues across the country and a map to find them — how the change turns Windsor into a multi-site system"
running_title: Change Analysis CR-02 — Windsor
author: General México Software Co.
date: 2026-11-06
lang: en
---

# The Change

**The client is buying new event venues around the country, including beach and mountain tourist areas, and asks for a map where Clients find a location and see the centers available in that city and area.**

The change has three parts that only work together:

- The **functional** part adds a location map to the Client's public site and a registry of centers for the Administrator.
- The **architecture** part splits the system into three parts: a Site node in each venue, a Client Service, and a Hall Administration. A Sync Service joins them.
- The **deployment** part moves the Client Service and the Hall Administration to the cloud. Each venue keeps its own LAN server, as Monterrey does today.

Budget ($1,200,000 MXN), timeline (6 months), milestones, and team stay as in v1.

The interview with the client fixed these points:

- **Where the map is.** The map lives only on the Client's public site.
- **Navigation.** It goes in two steps, country and then city. A city with one or more centers is an area; there is no state level.
- **What is drawn.** The country level is a drawing, not a geographic API. The city level shows each center with the free Google Maps Embed iframe.
- **What is shown.** Only centers with available dates appear. A center without dates is not shown.
- **Venue types.** The beach and mountain labels warn that those venues can lose their internet link.

Three assumptions complete the request:

- One company runs all venues with one system.
- The new venues open after the MVP (week 14).
- Between 5 and 10 venues operate.

The site hardware is bought by the client. The project only specifies it.

# Cost Analysis

The change costs $11,408 MXN in cash in the first year and every year after: 0.95% of the $1,200,000 MXN budget. Its effort is absorbed by the existing team, so it adds no payroll. The map uses the free Maps Embed API, so Google adds no cost.

## Recurring Cost

Table: Recurring cost of the change {#tbl-recurring}

| Item | Detail | Monthly | Year 1 | Year 2 onward |
|---|---|---|---|---|
| Cloud hosting | Two Vultr VPS for the Client Service, Hall Administration, Sync Service, and their PostgreSQL. Uses the "Full cloud" price of Table 12 in baseline v1 | — | $12,725 MXN | $12,725 MXN |
| Current Vultr proxy | The $3,291 MXN VPS proxy of v1 is replaced by the cloud hosting above | — | −$3,291 MXN | −$3,291 MXN |
| Docker Hub Pro | Private repositories for the four images: client, admin, sync, site; billed yearly | $9 USD | $1,974 MXN | $1,974 MXN |
| Google Maps Embed API | City map in an iframe; no charge and no usage limit | — | $0 MXN | $0 MXN |
| GitHub Actions | Builds and tests within the free allowance of the plan | — | $0 MXN | $0 MXN |
| **Total of the change** | Before taxes | — | **$11,408 MXN** | **$11,408 MXN** |

Object Storage is introduced by CR-03, and this change reuses it for photos and site backups. Sync traffic is under 1 GB per month, inside the transfer included with the VPS.

The hardware of each new venue is bought by the client and is outside the project budget. @tbl-hardware specifies the minimum:

- a mini PC with 4 cores, 16 GB RAM, and a 512 GB SSD;
- a UPS;
- an LTE router as a backup link for beach and mountain venues.

## One-Time Effort

The change takes 8 person-weeks:

- 4 for the Senior Developer: Sync Service, tunnels, deployment profiles, and rollout;
- 2 for the Mid-level Developer: the three-part split and the center registry;
- 1 for the Junior Developer: the map;
- 1 for the Project Manager: site hardware specification and rollout coordination.

At fixed salaries this time is worth $37,414 MXN. It is an internal cost of time, not a new expense.

Table: Minimum hardware of a new venue, bought by the client {#tbl-hardware}

| Item | Minimum | Purpose |
|---|---|---|
| Server | Mini PC, 4 cores, 16 GB RAM, 512 GB SSD | Runs the Site node and its PostgreSQL in containers |
| UPS | Enough for an orderly shutdown | Protects the database from power cuts |
| Router | Wired ISP link plus an LTE backup at beach and mountain venues | Keeps the tunnel to the cloud open when the main link fails |

## Position Against the Budget

The change is measured on the budget that CR-03 leaves, because CR-03 is implemented first (@fig-schedule). This follows the SCMP rule for coordinating changes that touch the same items.

Table: Position of the change against the budget {#tbl-budget}

| Concept | Amount | Share of budget |
|---|---|---|
| Total committed in baseline v1, first year | $587,468 MXN | 49.0% |
| Committed by CR-03 (AML module and Object Storage) | $63,948 MXN | 5.33% |
| Added by this change | $11,408 MXN | 0.95% |
| **Total committed, first year** | **$662,824 MXN** | **55.2%** |
| **Remaining against the $1,200,000 MXN budget** | **$537,176 MXN** | **44.8%** |

## Sensitivity

Table: Sensitivity of the first-year cost of the change {#tbl-sensitivity}

| Scenario | Year-1 cost of the change | Change vs base |
|---|---|---|
| Base (prices before taxes, about $18.28 MXN per USD) | $11,408 MXN | — |
| 16% IVA on Vultr and Docker Hub | $13,233 MXN | +$1,825 MXN |
| Peso weakens to $20 MXN per USD | $12,482 MXN | +$1,074 MXN |
| Both | $14,479 MXN | +$3,071 MXN |

Even in the worst case the change stays under 1.3% of the budget. The cost risk is the dependency on USD pricing, not the amount.

# How Impact Is Measured

The impact is rated with the five dimensions of CR-01: Time, Architecture, Money, People, and Modules. Each dimension is Low, Medium, or High under the same conditions.

A section of the baseline takes the highest level among the dimensions it touches. The overall impact is the middle value of the five ratings.

# What Changes from v1 to v2

The D-nn numbers are proposed. The Configuration Manager confirms them when the CCB approves the change.

Table: Sections of the baseline changed from v1 to v2 {#tbl-changed}

| Section | v1 | v2 | Decisions | Impact |
|---|---|---|---|---|
| General Information;<br>Justification and<br>Objective | One event venue with six halls in a single facility that share one LAN. | A company with several venues: Monterrey (six halls) plus beach and mountain venues, each with its own LAN. | D-14 (reverts D-13) | **Low.** Correction of the description. |
| Project Scope;<br>Functional<br>Requirements | 30 requirements. | Adds RF-SAL-006 "Center registry", RF-SAL-007 "Availability publication", and RF-CLI-005 "Location map". RF-AUTH-003 gains roles per venue. | D-15 | **High.** Modules: Event Halls, Client Management, and Authentication change. |
| Proposed<br>Technologies;<br>Architecture | One NestJS monolith on the LAN behind a Vultr proxy (Figure 2, Table 10). | Three parts from one repository: Site node (LAN), Client Service and Hall Administration (cloud), joined by a Sync Service. Four Docker images. Figures 2 and 3 and Table 10 are replaced by @fig-country and @fig-site. | D-16 | **High.** Architecture: the deployment and the layers change. |
| Non-Functional<br>Requirements | RNF-SEG-002 asks for a physically independent database server. | Adds RNF-DISP-002 "Site autonomy". RNF-SEG-002 lets new venues run the database in an isolated container on the same host. | D-17 | **High.** Architecture: a security requirement is relaxed for new venues. |
| Third-Party<br>Software | Vultr proxy $3,291 MXN per year. | Two Vultr VPS ($12,725 MXN), Docker Hub Pro ($1,974 MXN), Google Maps Embed ($0). Adds the minimum site hardware, bought by the client. | D-18 | **Low.** Money: 0.95% of the budget. |
| Agile Approach<br>and Schedule | Sprints 7–8: Email Notifications and integration; Sprints 9–12: hardening. | The three-part split and the Sync Service go in Sprints 7–8. The map, center registry, and site rollout kit go in Sprints 9–10. Milestones unchanged. | D-19 | **Medium.** Time: absorbed, but hardening gets less room. People: heavier load on the Senior Developer. |
| Identified Risks | 8 risks. | Adds data sync between sites and dependence on client hardware. The hall internet link risk extends to every venue. | D-20 | **Medium.** Architecture: two new risks. |

# Impact Assessment

Overall impact: **Medium**, the middle value of the five ratings in @tbl-impact (Low, Medium, Medium, High, High). The change is cheap and fits the schedule. It reshapes the architecture and puts the new infrastructure work, again, on the Senior Developer.

Table: Impact of the change by dimension {#tbl-impact}

| Dimension | Impact | Basis |
|---|---|---|
| Time | Medium | 8 person-weeks absorbed in Sprints 7–10. Hardening keeps weeks 19–26 but shares weeks 19–22 with the map and the rollout kit. The MVP (week 14) and the final release (week 26) hold. |
| Architecture | High | The monolith becomes three deployable parts plus a Sync Service. The Client Service and the Hall Administration move to the cloud, and RNF-SEG-002 changes for new venues. |
| Money | Low | $11,408 MXN in the first year, 0.95% of the budget. |
| People | Medium | Same team and salaries. The sync, tunnels, and rollout fall on the Senior Developer. No role covers distributed data or networking. |
| Modules | High | 3 of 7 modules gain or change requirements: Event Halls, Client Management, and Authentication. |

# Risk Measurement

The matrix of `artifacts/risk measurement.md` is applied as in CR-01. The assumptions of CR-01 are kept:

- 40 hours per person per week;
- 5,200 person-hours planned;
- 0 hours completed.

The budget is the $548,584 MXN that CR-03 leaves.

Table: Risk measurement of CR-02 {#tbl-risk-cr02}

| Metric | Measurement | Result | Level | Source |
|---|---|---|---|---|
| Effort | 8 person-weeks × 40 h = 320 h ÷ 5,200 h | 6.15% | **Medium** | This document, *One-Time Effort*; Baseline v1, *Project Team and Roles* |
| Budget | ($11,408 MXN in cash + $37,414 MXN of internal effort) ÷ $548,584 MXN remaining after CR-03 | 8.90% | **Low** | This document, @tbl-recurring and @tbl-budget |
| Time | 0 sprints added ÷ 13 sprints. The work is absorbed in Sprints 7–10, and both milestones hold | 0% | **No risk** | This document, @fig-schedule |
| Expertise | The least common skill is data sync between sites and multi-site VPN routing. No member lists it. The Senior Developer has related experience from the v1 proxy and tunnel | Partial coverage only | **Very high** | Baseline v1, *Project Team and Roles*; CR-01, *Risk Measurement* |
| Resource allocation | Vultr and Docker Hub are bought online and available the same day. The site hardware is bought by the client and has a delivery time, which fits because the venues open after the MVP | Delivery time fits the schedule | **Medium** | This document, @tbl-hardware |
| Legal compliance | The change is legal. The LFPDPPP privacy notice has to cover every venue; CR-03 publishes it | Yes | **No risk** | CR-03, *Risk Measurement* |

Overall risk: **Very high**, driven by Expertise, the same finding as in CR-01. It drops to High if the Senior Developer is trained on WireGuard with several peers and on the sync design before Sprint 7. It drops to Medium if the Mid-level Developer pairs on the Sync Service, which also gives the Senior Developer a backup.

# Architecture: Before and After

In v1 one monolith runs on the Monterrey LAN, and Clients reach it through the Vultr proxy.

In v2 the system is a star (@fig-country):

- **Client Service** (cloud): the public site, with the map, catalog, accounts, payments, and booking requests.
- **Hall Administration** (cloud): centers, halls, photos, prices, suppliers, roles, and reports, with internal access only.
- **Site node** (one per venue): runs on the venue's LAN, as in v1. It owns the bookings of its halls and validates QR codes at the door.

Venues never talk to each other. Each one opens an outbound WireGuard tunnel to the Sync Service, so no inbound port is opened on any LAN.

![Country view of the architecture: Clients on the internet reach Nginx on Vultr Mexico City over HTTPS. Nginx forwards to the Client Service (map, catalog, 360° tour viewer, accounts, payments, booking requests, AML module and API, email) and to the Hall Administration (centers, halls, prices, suppliers, photos and tours, roles, reports). Both use a Sync Service, which is the only gateway to the sites, plus a cloud PostgreSQL and Object Storage. The Monterrey, beach, mountain, and further sites each run a Site node with PostgreSQL and open outbound VPN tunnels to the Sync Service. The build pipeline runs GitHub Actions, then Docker Hub with four private images, then a staged rollout from QA to Monterrey to the other sites](../images/cr-02-architecture-country.svg "v2: country view; the tags name the change request that adds each part"){#fig-country}

A Site node keeps working without internet (@fig-site):

- desk bookings and QR validation continue on the LAN;
- an outbox in the sync agent queues what has to reach the cloud and sends it when the link returns.

![Site view of the architecture: inside the site LAN, on-site staff and security guards use the Site app in Docker (AuthModule, HallsModule, GuestsModule, and a sync agent with an outbox) on a client-owned mini PC. PostgreSQL runs in an isolated container, and a backup job sends encrypted copies to Object Storage. A UPS protects the server, and a router with an ISP link and an LTE backup carries one outbound WireGuard tunnel to the Sync Service in the cloud. The Sync Service receives availability, heartbeat, walk-in clients, and attendance, and sends guest lists, catalog, and booking requests. The site image is pulled from Docker Hub](../images/cr-02-architecture-site.svg "v2: inside a Site node"){#fig-site}

Every piece of data has one owner, which removes write conflicts between venues:

- Each Site node owns the bookings, availability, and blocks of its halls.
- The Hall Administration owns the catalog.
- The Client Service owns accounts and payments.

The availability the map shows is a copy that each venue publishes. The final word on a booking is the venue's: it checks for overlaps against its own database (RF-SAL-003). A stale copy can therefore never create a double booking.

# Map Flow

@fig-map-flow shows how a Client finds a center, and @tbl-map-flow describes each step.

![Map navigation flow in five steps on the public Client Service: 1, a country map drawn in SVG shows the cities with centers that have available dates; 2, the city view lists those centers with a Google Maps Embed iframe; 3, the center view shows its halls with photos and prices; 4, the hall view shows photos and a Virtual tour button added by CR-04; 5, the booking view of RF-CLI-004, where the site confirms the booking. The Hall Administration supplies centers, cities, coordinates, halls, and photos, and the Sync Service supplies the availability copy published by each site. Centers without available dates are not shown at any level](../images/cr-02-map-flow.svg "Map navigation flow"){#fig-map-flow}

Table: Map flow steps {#tbl-map-flow}

| Step | Screen | What the Client sees | Requirements |
|---|---|---|---|
| 1 | Country map | Cities that have at least one center with available dates | RF-CLI-005, RF-SAL-006 |
| 2 | City | The centers of the city with available dates, and the selected center on the Google Maps iframe | RF-CLI-005, RF-SAL-007 |
| 3 | Center | Its halls with photos and prices | RF-SAL-001, RF-SAL-004 |
| 4 | Hall | Photos and, after CR-04, the virtual tour | RF-SAL-004 |
| 5 | Booking | The booking view; the request goes through the Sync Service to the venue, which confirms it | RF-CLI-004, RF-SAL-003 |

# Schedule Impact

The work lands after the MVP, in the same block as CR-04, so the new screens are designed only once:

- the three-part split and the Sync Service in Sprints 7–8;
- the map, the center registry, and the site rollout kit in Sprints 9–10.

Opening each venue after that is an operations task, not development: install the site image and give it its configuration (CI-CFG-nn). The MVP stays at week 14 and the final release at week 26.

![Timeline of the 26-week sprint schedule with the baseline v1 rows in gray and two rows added by CR-02 in blue: the three-part split and the Sync Service in weeks 15 to 18, and the map, center registry, and site rollout kit in weeks 19 to 22; the MVP release stays at week 14 and the final release at week 26](../images/cr-02-schedule.svg "Sprint schedule; the blue rows carry the work added by this change"){#fig-schedule}

# Risks Added

Table: Risks added or extended by the change {#tbl-risks}

| Risk | Description | Impact |
|---|---|---|
| Data sync between sites | If the sync fails or lags, the map shows stale availability. Bookings stay safe because the venue confirms them, but Clients may see dates that are gone. | Medium |
| Client hardware | Each venue depends on hardware the client buys and installs. A late or undersized server delays the opening of that venue. | Medium |
| Hall internet link<br>(extended) | The risk now applies to every venue, and beach and mountain links are weaker. The venue keeps operating on its LAN, and the LTE backup reduces the outage. | Medium |
| Version drift | Venues may run different versions if a rollout stops halfway. Status accounting has to record the version of each venue. | Medium |

The change adds no people risk, but it makes the key-person dependency (High) heavier: the sync and the tunnels rest on the Senior Developer.

<div style="break-before: page"></div>

# SWOT Analysis (FODA) of the Change

Table: SWOT of the change, centered on the people of the project {#tbl-swot}

| | Helpful | Harmful |
|---|---|---|
| Internal<br>(project team) | <strong>Strengths</strong><ul><li>The three parts come from one repository and one pipeline, which the Senior Developer already runs.</li><li>The map needs Angular, which all three developers know.</li><li>The Project Manager designs the map screens in Figma together with the redesign of CR-04.</li><li>The team and salaries stay as in v1.</li></ul> | <strong>Weaknesses</strong><ul><li>Sync, tunnels, and rollout fall on the Senior Developer, with no backup.</li><li>No member lists distributed data or networking skills.</li><li>The team has no tester, and sync failures are hard to reproduce.</li><li>Sprints 7–10 carry this change and CR-04 on top of their planned work.</li></ul> |
| External<br>(client, venues,<br>Clients) | <strong>Opportunities</strong><ul><li>Clients find venues across the country in two clicks.</li><li>Each venue keeps operating when its link fails.</li><li>A new venue opens by installing the same image, with no development.</li></ul> | <strong>Threats</strong><ul><li>The client may buy or install the site hardware late.</li><li>Nobody at the new venues is named to look after the server and the link.</li><li>Resource reassignment (High): if the Senior Developer is moved during Sprints 7–10, the change stops.</li></ul> |

# Alternatives Analyzed

Eight ways of supporting several venues were considered. Four were scored, and the three-part split with availability-only sync ranked first with 3.45 of 5.

Four were discarded before scoring:

- **Two-way replication between all databases:** does not fit the schedule.
- **One microservice per module:** no member has that experience.
- **Load balancing across venues:** breaks the rule of one owner per piece of data and depends on the weakest links.
- **Google Maps JavaScript API:** charges per map load, while the Embed API is free.

The weights are those of CR-01: schedule fit 40%, budget 20%, security 15%, availability 15%, and impact on the baseline 10%.

Table: Multi-site alternatives evaluated {#tbl-alternatives}

| Alternative | Description | Annual cost | Effort (person-weeks) | Score |
|---|---|---|---|---|
| Three-part split with availability sync (selected) | Site node on each LAN; Client Service and Hall Administration in the cloud; venues publish only their availability | $14,699 MXN | 8 | **3.45** |
| VPS proxy per venue | The v1 design repeated at each venue; the cloud queries each venue live to draw the map | $3,291 MXN | 6 | 3.35 |
| Hybrid write/read per venue | Each venue's full database replicated to a read copy in the cloud (Table 12 of v1) | $11,848 MXN | 10 | 2.75 |
| Full cloud | All data and operation in the cloud; venues use it through the internet | $12,725 MXN | 6 | 2.60 |

The VPS proxy per venue comes close because it is cheap. It loses on availability: a venue without internet disappears from the map even when it has free dates, which is exactly what beach and mountain venues will suffer.

Full cloud scores lowest because a venue without internet could not even validate QR codes at the door.

# Change Request Form (F-01)

Table: Change request form of CR-02 {#tbl-f01}

| Section | Field | Value |
|---|---|---|
| Request | CR ID, title, date, requester | CR-02, "Venues across the country and location map", 2026-11-06, client (venue owner) |
| Request | Original request and reason | "The client is buying new hall centers around the country, including beach locations and tourist mountain areas. He is requesting a map to see the different locations. Once a location is selected, the system should display the center or centers available in the city and the area." |
| Analysis | Affected CIs | Baseline v1 (CI-DOC-02), requirements CSV (CI-REQ-01), Figures 1–4 of the baseline (CI-IMG-01, CI-IMG-03 to CI-IMG-05), monolith (CI-CODE-01), Nginx and WireGuard configuration (CI-CFG-01, CI-CFG-02); new CI-CODE-02 to CI-CODE-05, CI-DB-01, CI-DB-02, CI-CFG-03 to CI-CFG-05, CI-CFG-08, CI-CFG-09 |
| Analysis | Risk per metric and overall | Effort Medium, Budget Low, Time No risk, Expertise Very high, Resource allocation Medium, Legal No risk; overall **Very high** |
| Decision | Approved, rejected, or deferred | Pending CCB |
| Implementation | Branch, PRs, D-nn | `cr-02/multi-site-map`; proposed D-14 to D-20 |
| Verification | Date, verifier, result | Pending |
| Release | New versions | Baseline v2; release 2.0.0 |
| Closure | Date and archive | Pending |

State: **Analyzed**. Depends on CR-03, which introduces Object Storage.
