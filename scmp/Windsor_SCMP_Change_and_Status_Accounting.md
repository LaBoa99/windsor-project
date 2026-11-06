---
title: Implementing Change and Configuration Status Accounting
subtitle: "Software Configuration Management Plan (SCMP) sections for the “Windsor” Integrated Management System"
running_title: SCMP — Implementing Change and Status Accounting — Windsor
author: General México Software Co.
date: 2026-11-06
lang: en
---

[TOC]

# Purpose and Scope

This document defines two sections of the Software Configuration Management Plan (SCMP) of Windsor: how an approved change is verified and implemented, and how the status of baselines and changes is recorded and reported. For each one it gives the policies (the rules the team follows) and the formats (the forms, registers, and reports where the information is written).

It applies to every configuration item (CI) of the project: the baseline documents, the requirements, the diagrams, and, from Sprint 0 on, the source code, the database scripts, and the deployment configuration. The level of control is kept proportional to a five-person team with a 6-month timeline: every record lives in the project's Git repository, so no extra tool or licence is needed.

# Roles

The baseline names the team but not who controls changes. @tbl-roles assigns those duties to the existing members, so no one is hired.

Table: Configuration management roles {#tbl-roles}

| Role | Held by | Duties in this plan |
|---|---|---|
| Change Control Board (CCB) chair | General Director | Approves, rejects, or defers change requests; approves releases and new baselines. Decides on resources, which the Project Manager cannot do. |
| Configuration Manager (CCB secretary) | Project Manager | Keeps the change request register and the CI register, archives completed change requests, and issues the status accounting reports. |
| Technical member of the CCB | Senior Developer (lead) | Analyzes the impact of each change, plans releases, and runs the deployment pipeline. |
| Implementer | Any developer | Implements an approved change on its own branch and opens the pull request. |
| Verifier | A developer other than the implementer | Reviews the pull request and runs the tests. The team has no tester, so verification is always done by a second person. |
| Client validator | Venue staff, at the Sprint Review | Accepts or rejects the change as seen in the QA environment. |

A CCB decision needs the chair and at least one of the other two members. When the chair is absent, the decision waits; it is never taken by the Configuration Manager alone.

# Implementing Change

## Policies

Table: Change implementation policies {#tbl-ic-policies}

| ID | Policy |
|---|---|
| IC-01 | Only a change request (CR) in the **Approved** state can be implemented. Work started before approval is not merged. |
| IC-02 | Every CR moves through these states, and each state change is written in the CR register with its date: Submitted → Analyzed → Approved, Rejected, or Deferred → In implementation → Verified → Released → Closed. These states follow the change workflow: Request → Analysis → Decision → Development → Test → Delivery. Several approved CRs can be in Development at the same time, each on its own branch. |
| IC-03 | Each CR is implemented on its own branch named cr-nn/<short-name>. Every commit and pull request names the CR (for example, CR-01: add booking view). |
| IC-04 | A change is verified when it meets the Definition of Done: the pull request is reviewed by someone other than its author, unit and integration tests pass, and the increment is deployed to the QA environment. The client then validates it at the Sprint Review. The verifier and the date are written in the CR. |
| IC-05 | Every decision that changes the baseline gets a D-nn row in the Change History of the baseline document, and the CR lists those decisions. |
| IC-06 | The new version of every affected CI is identified before release (see @tbl-versions). A CR cannot be closed while any affected CI has no new version. |
| IC-07 | Only verified CRs go into a release. A release to production needs the approval of the CCB chair. |
| IC-08 | A completed CR is archived and never edited again. A correction is made with a new CR. |

## Version Identifiers

Table: Version identifiers {#tbl-versions}

| Item | Identifier | Example |
|---|---|---|
| Change request | CR-nn, numbered in order of submission | CR-01 |
| Baseline decision | D-nn, numbered across all versions | D-13 |
| Baseline | v{n}, numbered from v0, with a Git tag BL-v{n} | v1 |
| Configuration item | CI-<type>-nn plus its version and SHA-256 hash; types: DOC, REQ, IMG, CODE, DB, CFG | CI-DOC-03 v1 |
| Software release | MAJOR.MINOR.PATCH, with a Git tag. The MVP (week 14) is 1.0.0 and the final release (week 26) is 2.0.0 | 1.0.0 |
| Deviation | DEV-nn: an accepted difference between a CI and its baseline | DEV-01 |

## Archiving Completed Change Requests

- When a CR is closed, the Configuration Manager files the completed form (@tbl-cr-form) together with its change analysis in the change request archive of the project repository, one folder per CR.
- Rejected and deferred CRs are archived the same way, with the reason for the decision.
- Archived files are not edited or deleted. The Git history proves they have not changed.
- Archived CRs are kept for the life of the project and one year after the final release.

## Planning and Control of Releases

- Releases follow the sprint plan: the MVP release at the end of Sprints 5–6 (week 14) and the final release at the end of Sprints 9–12 (week 26). A CR that fixes a defect in production can go out as a patch release (for example, 1.0.1).
- At sprint planning, the Senior Developer proposes which verified CRs go into the next release, and the CCB approves the list.
- Every release goes first to the QA environment and is validated at the Sprint Review. Only then does the CCB chair approve it for production.
- Every release has a release note (@tbl-release-note) and a Git tag. The previous tag is kept so the release can be rolled back.

## Coordinating Multiple Changes

- All CRs are in one register, so the CCB sees every pending change at the same time.
- The CCB reviews the pending CRs together at each sprint planning, not one by one as they arrive.
- When two CRs affect the same CI, the CCB sets their order by priority. The second CR is analyzed again against the version the first one leaves, before it is implemented.
- CRs approved in the same period can go into one baseline version, as D-01 to D-13 went into v1.
- An urgent change (a security problem or a production outage) can be approved by the CCB chair and the Senior Developer and implemented at once. It is written in the register within 2 business days and reviewed at the next CCB meeting.

## Adding New Configuration Items

- A new CI enters the configuration only through an approved CR, which names it. For example, CR-01 added the deployment and booking-flow diagrams.
- The Configuration Manager gives it a CI-<type>-nn identifier and writes it in the CI register (@tbl-ci-register) with its owner, location, version, and SHA-256 hash.
- From then on, any change to it needs a CR.

## Delivering a New Baseline

1. Check that every CR included in the baseline is in the Verified state.
2. Check that the Change History has a D-nn row for every decision and that the requirements CSV matches the SRS.
3. Publish the baseline document with its new version number, v{n+1}.
4. Compute the SHA-256 hash of every CI and update the CI register.
5. Review the baseline against the Baseline Review Guidelines.
6. Get the CCB sign-off (chair and at least one member) and write it in the baseline.
7. Create the Git tag BL-v{n+1}, move the included CRs to Released, and send the Baseline Content Report (@tbl-reports).

## Formats

### Change Request Form

@tbl-cr-form lists the fields of a completed change request. The Configuration Manager fills in the identifiers and dates; the other fields are filled in by the person named in each row.

Table: Change request form (F-01) {#tbl-cr-form}

| Section | Field | Filled in by |
|---|---|---|
| Request | CR ID, title, date submitted, requester | Configuration Manager |
| Request | Original request, as written by the requester, and its reason | Requester |
| Analysis | Affected CIs, each with its name and current version | Senior Developer |
| Analysis | Risk level per metric (effort, budget, time, expertise, resource allocation, legal compliance) and overall level | Senior Developer |
| Decision | Approved, rejected, or deferred; date; reason; CCB members who signed | CCB |
| Implementation | Branch, pull requests, and baseline decisions (D-nn) | Implementer |
| Verification | Verification date, verifier, test result, and client validation at the Sprint Review | Verifier |
| Release | New version of each affected CI, new baseline or release identifier | Configuration Manager |
| Release | Release or installation date and responsible party | Senior Developer |
| Closure | Closing date and archive location | Configuration Manager |

### Release Note

Table: Release note (F-02) {#tbl-release-note}

| Field | Content |
|---|---|
| Release | Identifier and Git tag (for example, 1.0.0) |
| Date and responsible party | Release date; who deployed it; who approved it (CCB chair) |
| Environment | QA or production |
| CRs included | CR IDs and titles |
| CIs changed | CI IDs with their old and new versions |
| Known issues | Open defects and deviations (DEV-nn) |
| Rollback | Previous tag to return to |

# Configuration Status Accounting

## Definition and Objectives

Configuration status accounting records and reports the status of every CI under control, of the baselines, and of the changes made to them. A change is not applied the moment it is decided: it is analyzed, approved, implemented, verified, and released over several sprints. Status accounting keeps a record of where each change is at every moment and how the system has evolved from one baseline to the next, and it keeps the records that the configuration audits check.

Its objectives in Windsor are:

- To know at any moment which version of each CI makes up the current baseline.
- To know the state of every CR and how long it has been in that state.
- To find bottlenecks in time to act, such as CRs waiting for a decision or work piling up on one person.
- To give the configuration audits a complete and unchanged history.
- To show the quality of the Project Manager's work: the reports show whether changes were decided on time, whether the plan held, and whether bottlenecks were found and acted on.

## Why This Plan Fits Windsor

Each choice of the plan answers a risk or a gap of baseline v1:

Table: Project needs and how the plan answers them {#tbl-needs}

| Need of the project | Source | How the plan answers it |
|---|---|---|
| Scope had no formal change control | Identified Risks: unprotected scope (High) | Every change goes through a CR with a CCB decision (SA-02), and its state is reported every sprint. |
| Staff can be moved to other work without control | Identified Risks: resource reassignment (High) | The work tracking format (@tbl-work) shows who works on what, so a reassignment is visible at the next report. |
| The project rests on one person | Identified Risks: key-person dependency (High); CR-01 risk Very high (Expertise) | Tasks that only one person can do are flagged as bottlenecks. |
| The Project Manager tracks status but does not decide on resources | Identified Risks: PM authority (Med-high) | The Project Manager reports; the CCB chair, who decides on resources, receives the reports and acts on them. |
| Third-party services have prices and renewals in USD | Identified Risks: third-party dependency (Medium) | Contracted services are CIs with their renewal dates. |
| A five-person team with no CM tool | Project Team and Roles | The Git repository and two registers replace a CMDB, at no cost. |

## Policies

Table: Status accounting policies {#tbl-sa-policies}

| ID | Policy |
|---|---|
| SA-01 | The Git repository is the single source of status data. Nothing is official if it is not in the repository. |
| SA-02 | A change is official only when the CCB approval is written in the CR register. |
| SA-03 | Whoever causes an event (a CR is submitted, decided, merged, verified, or released) updates the register the same working day, in a commit that names the CR. |
| SA-04 | Records are never deleted or rewritten. A mistake is corrected with a new row that references the wrong one. |
| SA-05 | Reports are generated from the registers, not written by hand, so the report and the data always match. |
| SA-06 | Every report carries its date, the baseline it refers to, and who prepared it. |

## What Is Tracked and Reported

Table: Elements tracked for baselines and changes {#tbl-tracked}

| Element | Data recorded | Where it is recorded |
|---|---|---|
| Baselines | Version, date, CIs included with their versions and hashes, CRs included, sign-off | CI register; Change History; Git tag |
| Configuration items | ID, name, type, version, owner, location, SHA-256 hash, state (Draft, Under review, Baselined, Superseded) | CI register |
| Change requests | ID, title, state and date of each state change, risk level, affected CIs, CCB decision, decisions D-nn, pull requests | CR register; CR form |
| Approvals | Who approved what and when (CRs, releases, baselines) | CR register; baseline sign-off |
| Deviations | ID, CI, how it differs from the baseline, reason, whether it was accepted, and the CR that will resolve it | CR register, as DEV-nn rows |
| Releases | Identifier, date, environment, CRs included, who deployed and who approved | Release notes; Git tags |
| Effort | Estimated and actual person-hours per CR, and who works on it | CR register |
| Budget | Cash cost committed per CR, against the $1,200,000 MXN budget | CR register |
| Quality | Tests passed and failed per pull request; defects found in QA and production | Pipeline results; CR register |
| Infrastructure | Contracted third-party services and their renewal dates (Vultr, Porkbun, SendGrid, Facturama) | CI register, as CFG items |

## Reports and Frequency

Table: Status accounting reports {#tbl-reports}

| Report | Content | Frequency | Prepared by | Received by |
|---|---|---|---|---|
| CR Status Report | Every open CR with its state, days in that state, risk level, and owner | Every sprint (2 weeks), before the Sprint Review | Configuration Manager | CCB; team |
| Baseline Content Report | CIs of the baseline with their versions and hashes, and the CRs it includes | At each new baseline | Configuration Manager | CCB; client |
| Change History | One row per decision D-nn | On every decision, inside the baseline document | Configuration Manager | Everyone who reads the baseline |
| Release Report | Release note (@tbl-release-note) | At each release | Senior Developer | CCB; client |
| Project Monitoring Report | Effort and cost of changes against estimates; CRs per person; tests and defects; bottlenecks | Every sprint, with the CR Status Report | Configuration Manager | CCB |
| Audit Report | Full history of the CIs and CRs being audited | On request, before each configuration audit | Configuration Manager | Auditor; CCB |

### Work Planning

The Project Monitoring Report is used to plan the work of the next sprint. It shows what each person is working on and how it turned out (@tbl-work), and it monitors these development variables: velocity (user stories completed per sprint), open CRs per state, tests passed against tests run, and actual effort against the estimate.

Table: Work tracking (F-05) {#tbl-work}

| Column | Content |
|---|---|
| Person | Team member |
| Task | CR or user story, and what is being done (for example, CR-01: Nginx proxy and VPN tunnel) |
| State | Workflow step: Analysis, Development, Test, or Delivery |
| Days in state | Days since the task entered its current state |
| Effort | Estimated and actual person-hours |
| Result | Test result (passed or failed) and whether the client accepted it at the Sprint Review |
| Bottleneck | Yes or No, with the reason |

A row is marked as a bottleneck when:

- the task stays in the same state for more than one sprint,
- the person has more than two open CRs at once,
- the actual effort exceeds the estimate by more than 25%, or
- the task can be done by only one person (for example, the proxy and tunnel work of CR-01, which rests on the Senior Developer).

The CCB uses the bottlenecks at sprint planning to move tasks between people or to defer a CR.

## How Information Is Collected, Stored, and Reported

- **Collection.** Data is captured when each event happens (@tbl-events), by the person responsible for the event.
- **Storage.** Two registers in the project repository act as the configuration database: the CR register (@tbl-cr-register) and the CI register (@tbl-ci-register). Completed CRs go to the change request archive. The repository is hosted on GitHub, and every team member keeps a local clone, which also serves as a backup.
- **Reporting.** The Configuration Manager generates each report from the registers and stores it in the project repository.

Table: Events that update the records {#tbl-events}

| Event | Record updated | By |
|---|---|---|
| A CR is submitted | New row in the CR register (state Submitted) | Configuration Manager |
| A CR is analyzed | Risk level and affected CIs | Senior Developer |
| The CCB decides | State, decision date, and signatures | Configuration Manager |
| A pull request is merged | State In implementation, and the pull request | Implementer |
| A change is verified | State Verified, verifier, and date | Verifier |
| A release or baseline is published | State Released, new CI versions and hashes, Git tag | Senior Developer; Configuration Manager |
| A CR is closed | State Closed; the CR goes to the change request archive | Configuration Manager |

### Register Formats

Table: CR register columns (F-03) {#tbl-cr-register}

| Column | Content |
|---|---|
| ID | CR-nn or DEV-nn |
| Title | Short title |
| State | Submitted, Analyzed, Approved, Rejected, Deferred, In implementation, Verified, Released, Closed |
| State date | Date of the last state change |
| Requester | Who asked for the change |
| Risk | Overall risk level from the risk metrics matrix |
| Affected CIs | CI IDs with their current versions |
| Decisions | D-nn rows in the Change History |
| Owner | Person implementing the change |
| Estimated and actual effort | Person-hours |
| Cost | Cash cost of the change in MXN |
| New version | Baseline or release that includes the change |

Table: CI register columns (F-04) {#tbl-ci-register}

| Column | Content |
|---|---|
| ID | CI-<type>-nn |
| Name | Name of the item |
| Location | Place in the project repository, or the supplier for a contracted service |
| Version | Current version |
| Date | Date of the current version |
| SHA-256 | Hash of the current version |
| Owner | Person responsible for the item |
| State | Draft, Under review, Baselined, Superseded |

## How Access Is Controlled

The repository is private and only the five team members have access. The main branch is protected: changes go in only through a reviewed pull request.

Table: Access to status data {#tbl-access}

| Who | View | Update | Review | Approve |
|---|---|---|---|---|
| CCB chair (General Director) | Yes | No | Yes | CRs, releases, baselines |
| Configuration Manager (Project Manager) | Yes | Registers and reports | Every change to the registers | No |
| Senior Developer | Yes | Analysis, releases, CI versions | Pull requests | Urgent changes, with the chair |
| Mid-level and Junior Developers | Yes | Their own CRs (implementation and verification) | Pull requests of others | No |
| Client (venue) | Reports sent to them only | No | Sprint Review | Acceptance of changes in QA |

Changes to the registers and to the change request archive need the Configuration Manager's review before they are merged. Archived CRs and published reports are read-only.
