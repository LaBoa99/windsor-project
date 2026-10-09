---
title: Change Analysis CR-01
subtitle: "Online hall booking for Clients — how the change reshaped the Windsor baseline from v0 to v1"
running_title: Change Analysis CR-01 — Windsor
author: General México Software Co.
date: 2026-10-09
lang: en
---

# The Change

**Clients can browse the hall catalog, check availability, and book an available slot on their own, over the internet and at any time, without visiting the venue.**

The change has two parts that only work together. The **functional** part adds requirement RF-CLI-004, "Hall booking view", to Client Management. The **deployment** part makes the system reachable at `saloneswindsor.mx` through a public Nginx proxy on a Vultr VPS, linked to the venue's LAN by a VPN tunnel. Budget ($1,200,000 MXN), timeline (6 months), milestones, and team stay as in v0.

# Cost Analysis

The change costs $3,941 MXN in cash during the first year and $4,045 MXN per year after, 0.33% of the $1,200,000 MXN budget. Its effort is absorbed by the existing team, so it adds no payroll.

## Recurring Cost

Table: Recurring cost of the change {#tbl-recurring}

| Item | Detail | Monthly | Year 1 | Year 2 onward |
|---|---|---|---|---|
| Vultr instance | Cloud Compute Regular Performance `vc2-1c-1gb`, Mexico City: 1 vCPU, 1 GB RAM, 25 GB SSD, 1 TB transfer | $5 USD | $1,097 MXN | $1,097 MXN |
| Vultr DDoS protection | 10 Gbps of mitigation on the public IP address | $10 USD | $2,194 MXN | $2,194 MXN |
| Porkbun domain | `saloneswindsor.mx` with DNS and SSL certificate included; $35.57 USD the first year, $41.23 USD on renewal | — | $650 MXN | $754 MXN |
| WireGuard VPN tunnel | Open source, runs on the VPS and the LAN server | — | $0 MXN | $0 MXN |
| **Total of the change** | Before taxes | **$15 USD** | **$3,941 MXN** | **$4,045 MXN** |

Backups, snapshots, and an extra IPv4 are not contracted: the proxy stores no data and its configuration lives in the repository. Extra transfer above 1 TB per month would cost $0.01 USD per GB.

## One-Time Effort

The baseline estimates only the public access (proxy, tunnel, DNS, and SSL): about 1 person-week, the effort of the selected alternative in @tbl-alternatives. At the Senior Developer's salary of $23,000 MXN per month (about 4.33 weeks), that week is worth about $5,312 MXN. Salaries are fixed, so this is an internal cost of time, not a new expense.

The booking view (RF-CLI-004) has no separate estimate in the baseline. It is one more requirement of Client Management in Sprints 3–4, which go from 7 to 8 requirements against 9 in the neighboring sprint pairs.

## Position Against the Budget

Table: Position of the change against the budget {#tbl-budget}

| Concept | Amount | Share of budget |
|---|---|---|
| Total project team cost with 25% profit (unchanged from v0) | $577,500 MXN | 48.1% |
| Third-party costs in v0 (Facturama) | $1,650 MXN | 0.14% |
| Third-party costs added by this change (Vultr and domain) | $3,941 MXN | 0.33% |
| SendGrid, budgeted in the same revision (D-08) | $4,377 MXN | 0.36% |
| **Total committed, first year** | **$587,468 MXN** | **49.0%** |
| **Remaining against the $1,200,000 MXN budget** | **$612,532 MXN** | **51.0%** |

## Sensitivity

Table: Sensitivity of the first-year cost of the change {#tbl-sensitivity}

| Scenario | Year-1 cost of the change | Change vs base |
|---|---|---|
| Base (prices before taxes, about $18.28 MXN per USD) | $3,941 MXN | — |
| Vultr charges 16% IVA | $4,468 MXN | +$527 MXN |
| Peso weakens to $20 MXN per USD | $4,311 MXN | +$370 MXN |
| Both | $4,887 MXN | +$946 MXN |

Even in the worst case the change stays under 0.5% of the budget, so the cost risk is the dependency on USD pricing, not the amount.

# How Impact Is Measured

The impact of the change is rated Low, Medium, or High on five dimensions. @tbl-criteria gives what is measured in each dimension and the condition for each level.

Table: Criteria for rating the impact in each dimension {#tbl-criteria}

| Dimension | What is measured | Low | Medium | High |
|---|---|---|---|---|
| Time | Effect on the sprint plan and on the two milestones (MVP at week 14, final release at week 26) | The work is absorbed in the sprints already planned; no sprint goal or milestone moves. | A sprint goal moves to another sprint, but both milestones hold. | A milestone moves, or the 6-month timeline grows. |
| Architecture | Effect on the layers and components of the architecture, and on where the system runs and how it is reached | No layer, component, or deployment changes; documentation only. | A component is added or changed inside an existing layer; the deployment stays the same. | The deployment changes (where the system runs or how it is reached), or a layer is added or removed. |
| Money | Cash cost of the first year as a share of the $1,200,000 MXN budget | Under 1% of the budget. | From 1% to 5% of the budget, covered by the remaining budget. | Over 5% of the budget, or the budget has to grow. |
| People | Effect on the team: members, roles, salaries, and workload | Same team; no role or workload changes. | Same team and salaries, but work is added to a role or a sprint, or the work needs a skill that no role covers. | A member is hired, replaced, or reassigned, or a salary changes. |
| Modules | Number of the 7 modules whose requirements are added or changed | No module gains or changes a requirement. | 1 or 2 modules gain or change requirements; the others are only called. | 3 or more modules gain or change requirements, or a module is added or removed. |

Two more rules apply. A section of the baseline (@tbl-changed) takes the highest level among the dimensions it touches. The overall impact of the change (@tbl-impact) is the middle value of its five ratings.

# What Changed from v0 to v1

Each row of @tbl-changed maps a section of the baseline to the decisions recorded in the v1 change history. Its impact is rated with the criteria of @tbl-criteria.

Table: Sections of the baseline changed from v0 to v1 {#tbl-changed}

| Section | v0 | v1 | Decisions | Impact |
|---|---|---|---|---|
| General Information;<br>Justification and<br>Objective | Chain of event venues in the Monterrey area. | One event venue with six halls in a single facility that share one LAN. | D-13 | **Low.** Correction of the description; no effect on time, money, people, or modules. |
| Project Scope | Client Management: profile, event history, event panel. | Adds the hall booking view. | D-01 | **Medium.** Modules: Client Management gains a feature. Time: absorbed in Sprints 3–4. |
| Functional<br>Requirements | 29 requirements; no booking screen for the Client. | 30 requirements: new RF-CLI-004 "Hall booking view". The Client Management overview describes it. | D-01 | **Medium.** Modules: one new requirement; the booking flow calls five other modules without changing them. |
| Third-Party<br>Software | Facturama only, $1,650 MXN per year. | Adds Vultr VPS with DDoS protection, Porkbun domain and SSL, and SendGrid. First year: $9,968 MXN. | D-04, D-06, D-08, D-11 | **Low.** Money: this change adds $3,941 MXN, 0.33% of the budget. Two new vendors. |
| Proposed<br>Technologies | Docker (deployment place not defined). | Docker on the client's on-premises LAN, published through Nginx on a Vultr `vc2-1c-1gb` VPS over a VPN tunnel. | D-03, D-06 | **High.** Architecture: stack unchanged, but the deployment changes: hosting adds a VPS and a tunnel. People: the infrastructure work falls to the Senior Developer (lead). |
| Agile Approach<br>and Schedule | Sprints 3–4: Client Management; Suppliers and Services. | Sprints 3–4 also carry the booking view and its public access. Adds a timeline figure. Milestones unchanged. | D-02, D-10 | **Medium.** Time: about 1 person-week for the public access plus one more requirement, inside Sprints 3–4; milestones unchanged. People: heavier sprint load for the same team. |
| Identified Risks | 6 risks; third-party dependency covered Facturama only. | 8 risks: adds internet exposure and hall internet link; third-party dependency extends to SendGrid, Vultr, and Porkbun. | D-05, D-08, D-11 | **Medium.** Architecture: two new risks, one rated Med-high. People: no risk has an owner, and the people risks of v0 were not updated. |
| Architecture | One diagram; a connector joined the application directly to the presentation layer. | Diagram redrawn with the same layers and components, without that connector (it contradicted RNF-SEG-001); the deployment paragraph describes the VPS and the tunnel. | D-03, D-06, D-09, D-11, D-13 | **High.** Architecture: deployment moves from LAN-only to an internet-facing proxy with a VPN tunnel. |
| Communication | Not defined. | Two new figures: deployment communication and the booking-flow module messages. | D-07 | **Low.** Documentation only. |
| Assumptions<br>and Notes | Present. | Removed. | D-12 | **Low.** Documentation only. |

D-08 (SendGrid cost), D-09 (diagram redrawn), D-10 (timeline figure), and D-12 (removal of Assumptions and Notes) were made in the same revision but do not depend on this change. All 6 non-functional requirements are unchanged.

# Impact Assessment

Overall impact: **Medium**, the middle value of the five ratings in @tbl-impact (Low, Low, Medium, Medium, High). The change is cheap and fits the schedule, but it reshapes the deployment architecture and puts new infrastructure work on the same five people, mostly on the Senior Developer (lead).

Table: Impact of the change by dimension {#tbl-impact}

| Dimension | Impact | Basis |
|---|---|---|
| Time | Low | About 1 person-week for the public access and one more requirement, absorbed in Sprints 3–4. The MVP stays at week 14 and the final release at week 26; the 6-month timeline is unchanged. |
| Architecture | High | Deployment changes from LAN-only to a public Nginx proxy on Vultr with a WireGuard tunnel, DNS, and SSL. The NestJS monolith, PostgreSQL, RNF-SEG-001, and RNF-SEG-002 stay as they were. |
| Money | Low | $3,941 MXN in the first year ($4,045 MXN from the second), 0.33% of the $1,200,000 MXN budget. The budget is unchanged. |
| People | Medium | Same team and salaries, no new hires. The proxy and the tunnel fall to the Senior Developer (lead), the only member whose role covers the pipeline and Docker and already a key-person risk. No role covers networking, security, or testing, and Sprints 3–4 carry more work. @tbl-swot details the people side. |
| Modules | Medium | 1 of 7 modules gains a requirement (Client Management, RF-CLI-004). The booking flow calls Authentication, Event Halls, Suppliers, Purchases, and Email Notifications without changing their requirements; Guests and QR Access is untouched. |

# Deployment: Before and After

In v0 the system lives on the hall LAN and Clients outside have no route to it (@fig-deployment-v0). In v1 Clients reach the system only through the Vultr VPS; the LAN opens the tunnel outbound, so no inbound port is exposed and the monolith and database never face the internet (@fig-deployment-v1).

![Deployment diagram of v0 with two zones: in the internet zone, the Client on a browser or phone has no public route to the system; in the hall LAN zone, the Nginx proxy with SSL and backend hiding connects to the NestJS monolith on port 3000, which connects to PostgreSQL on port 5432, and staff and security guards reach the monolith over the local network](../images/cr-01-deployment-v0.svg "v0: LAN-only deployment, with no public route for Clients"){#fig-deployment-v0}

![Deployment diagram of v1 with three zones: in the internet zone, the Client looks up saloneswindsor.mx in Porkbun DNS and connects over HTTPS 443 to the Vultr VPS vc2-1c-1gb in Mexico City, which runs Nginx with SSL and DDoS protection; the VPS forwards requests through a WireGuard VPN tunnel, opened outbound by the LAN, to the NestJS monolith on port 3000 in the hall LAN, which has no inbound ports open; the monolith connects to PostgreSQL on port 5432, staff and security guards reach it over the local network, and it sends email to SendGrid over SMTP](../images/cr-01-deployment-v1.svg "v1: public proxy on Vultr with a VPN tunnel to the hall LAN"){#fig-deployment-v1}

# Booking Flow

v0 did not define calls between modules. The flow in @fig-booking-flow is derived from the requirements and shows what happens when a Client books a hall through RF-CLI-004; @tbl-booking-flow describes each step.

![Module communication diagram of the booking flow with numbered messages: 1, the Client browser sends a request to Nginx on the Vultr VPS; 2, Nginx forwards it to AuthModule; 3, AuthModule passes it to HallsModule; 4, HallsModule calls PurchasesModule; 5, SupplierModule provides service prices to PurchasesModule; 6, PurchasesModule calls MailModule; 7, MailModule sends the email through SendGrid; a dashed arrow from GuestsModule to MailModule marks QR invitations, outside the booking flow, and all modules persist their data in PostgreSQL](../images/cr-01-booking-flow.svg "Module communication for the hall booking flow"){#fig-booking-flow}

Table: Booking flow messages {#tbl-booking-flow}

| Step | From → To | Message | Requirements |
|---|---|---|---|
| 1 | Client → Nginx | HTTPS request from the booking view | RF-CLI-004 |
| 2 | Nginx → AuthModule | Validate the JWT token and the Client role | RF-AUTH-002, RF-AUTH-003 |
| 3 | AuthModule → HallsModule | Look up availability and record the booking without overlap | RF-SAL-002, RF-SAL-003 |
| 4 | HallsModule → PurchasesModule | Calculate the total cost and create the purchase order | RF-COMP-001, RF-COMP-002 |
| 5 | SupplierModule → PurchasesModule | Provide the prices of the supplier services linked to the booking | RF-PROV-002 |
| 6 | PurchasesModule → MailModule | Request the booking confirmation email | RF-MAIL-001 |
| 7 | MailModule → SendGrid | Deliver the email over SMTP | RF-MAIL-001 |

# Schedule Impact

The new work lands in Sprints 3–4 (weeks 7–10), which carried 7 requirements against 9 in the neighboring sprint pairs, and Event Halls is already delivered by then. The MVP stays at week 14 and the final release at week 26.

![Timeline of the sprint schedule over 26 weeks and sprints S0 to S12: base architecture, CI/CD, data model, and backlog in weeks 1 to 2; Authentication and Access Control and Event Halls in weeks 3 to 6; Client Management with the hall booking view, public access through the Vultr proxy and VPN tunnel, and Suppliers and Services in weeks 7 to 10; Purchases and Payments and Guests and QR Access Control in weeks 11 to 14, ending with the MVP release; Email Notifications and cross-module integration in weeks 15 to 18; hardening, testing, UAT, training, and delivery in weeks 19 to 26, ending with the final release; the two rows for the booking view and the public access are highlighted in blue](../images/cr-01-schedule.svg "Sprint schedule; the blue rows carry the work added by this change. Client Management was already planned in v0, and the change adds the booking view to it"){#fig-schedule}

# Risks Added

Table: Risks added or extended by the change {#tbl-risks}

| Risk | Description | Impact |
|---|---|---|
| Internet exposure | The system is reachable from the internet through the public proxy, which widens the attack surface. | Med-high |
| Hall internet link | Online booking depends on the hall's internet link. If it fails, Clients cannot reach the system, although on-site operation continues on the LAN. | Medium |
| Third-party dependency<br>(extended) | Besides Facturama, email delivery depends on SendGrid and public access on Vultr and Porkbun; their prices or availability can change. | Medium |

The three risks above are about technology and vendors. The change added no people risk to the baseline, although it makes the people risks already there heavier: key-person dependency (High), resource reassignment (High), and PM authority (Med-high). @tbl-swot covers them.

<div style="break-before: page"></div>

# SWOT Analysis (FODA) of the Change

Table: SWOT of the change, centered on the people of the project {#tbl-swot}

| | Helpful | Harmful |
|---|---|---|
| Internal<br>(project team) | <strong>Strengths</strong><ul><li>The booking view needs no hiring or training: the three developers work with Angular, and the Senior and Mid-level Developers with NestJS and PostgreSQL.</li><li>The Senior Developer (lead) already builds the pipeline and works with Docker, so the proxy and the tunnel fit an existing role.</li><li>The Project Manager also does the UI/UX design in Figma, so the booking view is designed inside the team.</li><li>The team stays as in v0: the same five members and a monthly team cost of $77,000 MXN.</li></ul> | <strong>Weaknesses</strong><ul><li>The proxy, tunnel, DNS, and SSL fall to the Senior Developer (lead), on whom the project already rests (key-person dependency, High). Nobody is named as backup.</li><li>No member lists networking, VPN, or security skills, and the team has no infrastructure or security role, although the change exposes the system to the internet.</li><li>The team has no tester: the developers who build the booking view and the public access also test them.</li><li>The Project Manager tracks status but does not decide on resources (PM authority, Med-high), so the PM cannot guarantee the extra time in Sprints 3–4.</li><li>Sprints 3–4 go from 7 to 8 requirements plus the infrastructure work, with the same people.</li></ul> |
| External<br>(venue, Clients,<br>rest of the company) | <strong>Opportunities</strong><ul><li>Clients book on their own, at any time, without visiting the venue.</li><li>The venue's staff can spend less time taking bookings in person.</li><li>The Sprint Review with the client every 2 weeks lets the venue's staff validate the booking view before the MVP release.</li><li>The training planned in Sprints 9–12 can cover the booking view and what to do when the internet link fails.</li></ul> | <strong>Threats</strong><ul><li>Resource reassignment (High): staff can be moved to other work of the group without change control. If the Senior Developer is moved during Sprints 3–4, both parts of the change stop.</li><li>Client concentration (Med-high): a delay from another client can take the team's time in the sprints that carry the change.</li><li>The baseline names nobody at the venue to look after the LAN server and the internet link, on which online booking now depends.</li><li>Some Clients may keep booking in person, so the venue's staff would have to handle both ways.</li><li>Outside attackers can now reach the public proxy (internet exposure, Med-high), and responding falls on a team without a security role.</li></ul> |

# Alternatives Analyzed

Twelve ways of giving Clients access from outside the LAN were considered; six were scored, and the VPS proxy with a VPN tunnel ranked first with 4.55 of 5.

The other six were discarded before scoring: a LAN-only kiosk and a public request form do not let the Client book online in real time; a separate booking portal and bidirectional database replication do not fit the schedule; exposing the LAN server directly is not secure; and a managed platform had no verified prices.

The six remaining alternatives were scored from 1 to 5, weighting schedule fit 40%, budget 20%, security 15%, availability 15%, and impact on the baseline 10%.

Table: Deployment alternatives evaluated {#tbl-alternatives}

| Alternative | Description | Annual cost | Effort (person-weeks) | Score |
|---|---|---|---|---|
| VPS proxy (selected) | Public Nginx proxy on a Vultr VPS with a VPN tunnel to the LAN | $3,291 MXN | 1 | **4.55** |
| Cloudflare Tunnel | Tunnel from the LAN to Cloudflare, without a VPS | $0 MXN | 0.5 | 4.45 |
| Full cloud | Application and database on two Vultr VPS | $12,725 MXN | 1 | 3.75 |
| Managed database | Full cloud with a managed PostgreSQL database | $10,751 MXN to $27,206 MXN (unverified) | 1 | 3.70 |
| Hybrid direct | Application on a VPS using the LAN database over a VPN | $7,460 MXN | 2 | 3.10 |
| Hybrid write/read | Primary database on the LAN; read replica and application on Vultr | $11,848 MXN | 3.5 | 2.90 |

The annual cost counts hosting only. The $3,291 MXN of the selected alternative is the Vultr cost; with the domain ($650 MXN the first year) it becomes the $3,941 MXN of @tbl-recurring. The effort counts the public access only, not the booking view, which every alternative needs.

This SWOT looks at the people of the project: the development team, the venue's staff, and the Clients who book. The team can deliver the change with the people and skills it already has. Its weak side is that the new work lands on the Senior Developer (lead), with no backup and with no role for networking, security, or testing.

The selected alternative fits in Sprints 3–4, leaves RNF-SEG-001 and RNF-SEG-002 unchanged, and keeps the layers and components of the architecture diagram. The hybrid write/read alternative reuses the same VPS and tunnel, so it remains the next step if the hall's internet link proves unreliable.
