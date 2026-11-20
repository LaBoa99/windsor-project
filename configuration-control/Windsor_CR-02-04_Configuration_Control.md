---
title: Configuration Control CR-02 to CR-04
subtitle: "CCB decision on anti-money-laundering reporting, venues across the country, and a virtual tour"
running_title: Configuration Control CR-02 to CR-04 — Windsor
author: General México Software Co.
date: 2026-11-19
lang: en
---

# Risk Ranking

Each level of the risk metrics matrix (CI-DOC-05) is turned into a number, and the six metrics of each change are averaged.

Table: Numeric value of each risk level {#tbl-scale}

| Level | No risk | Low | Medium | High | Very high | Prohibitive |
|---|---|---|---|---|---|---|
| Value | 0 | 1 | 2 | 3 | 4 | 5 |

Table: Risk of each change and its average {#tbl-ranking}

| Rank | CR | Effort | Budget | Time | Expertise | Resource allocation | Legal compliance | Average | Effort (person-weeks) |
|---|---|---|---|---|---|---|---|---|---|
| 1 | CR-02 | Medium (2) | Low (1) | No risk (0) | Very high (4) | Medium (2) | No risk (0) | **1.50** | 8 |
| 2 | CR-03 | Low (1) | Medium (2) | No risk (0) | Very high (4) | Medium (2) | No risk (0) | **1.50** | 4 |
| 3 | CR-04 | Low (1) | Low (1) | No risk (0) | High (3) | Medium (2) | No risk (0) | **1.17** | 3 |

CR-02 and CR-03 tie at 1.50. The tie is broken by effort: CR-02 needs twice the person-weeks of CR-03. The ranking from most to least risky is **CR-02, CR-03, CR-04**.

# Decision

Table: CCB decision on each change request {#tbl-decision}

| CR | Decision | Reason |
|---|---|---|
| CR-03 | **Approved** | Mandatory: the anti-money-laundering law is in force, and the MVP takes bookings from week 14. |
| CR-04 | **Approved** | Lowest risk and within the team's resources. It runs on the monolith and stores its images in the Object Storage of CR-03. |
| CR-02 | **Rejected** | See *Why CR-02 Is Rejected*. Offered to the client as a phase 2 after the final release (week 26). |

Signed on 2026-11-19 by the General Director (CCB chair) and the Senior Developer.

# Why CR-02 Is Rejected

Table: Reasons for rejecting CR-02 {#tbl-reasons}

| # | Reason | What it breaks |
|---|---|---|
| 1 | **It lowers an approved security requirement.** RNF-SEG-002 asks for a physically independent database server. CR-02 lets the database of the new venues run in a container on the same host (D-17). | The security level the client approved in baseline v1. |
| 2 | **The team cannot verify it.** The team has no tester, and sync failures between sites are hard to reproduce. | The Definition of Done (SCMP, policy IC-04). |
| 3 | **Nobody on the team has the skill.** Expertise is Very high: no member lists distributed data or multi-site networking, and all the work rests on the Senior Developer, with no backup. | The delivery of the project if the Senior Developer is moved during Sprints 7–10. |
| 4 | **It takes time from hardening.** The map and the rollout kit share weeks 19–22 with hardening, just before the final release. | The quality of release 2.0.0. |
| 5 | **It depends on things the project does not control.** Each venue needs hardware the client buys and installs, and nobody at the new venues is named to look after it. | The schedule: a late or undersized server delays a venue. |
| 6 | **It is not urgent.** The new venues open after the MVP, and the number of venues (5 to 10) is still an assumption. | Nothing: moving it to a phase 2 costs the client no booking today. |
