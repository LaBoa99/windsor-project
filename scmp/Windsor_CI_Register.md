---
title: Configuration Item Register
subtitle: "SCMP form F-04 for the “Windsor” Integrated Management System, with the items of CR-02, CR-03, and CR-04 and the CCB decision on them"
running_title: SCMP — Configuration Item Register — Windsor
author: General México Software Co.
date: 2026-11-19
lang: en
---

[TOC]

# Purpose

This register lists every configuration item (CI) of the Windsor project, with its version, date, hash, owner, and state. It uses the columns of form F-04 in the SCMP (*Implementing Change and Configuration Status Accounting*). It adds an "Affected by" column that links each item to the change requests that touch it.

Two kinds of item appear:

- **Items that exist in the repository.** Their version and date come from Git.
- **Items that the change requests plan.** They are code, databases, and configuration that do not exist yet. They carry no hash, and they enter the project only when the CCB approves their change request (SCMP, *Adding New Configuration Items*).

The Configuration Manager (Project Manager) keeps this register. Every change to it goes through a pull request reviewed by the Configuration Manager (SCMP, *How Access Is Controlled*).

# How the Columns Are Filled

- **ID:** `CI-<type>-nn`, with the types of the SCMP: DOC, REQ, IMG, CODE, DB, CFG.
- **Version:** `v{n}`, where n is the number of commits that changed the file, so that anyone can check it with `git log --follow`. Items not committed yet are `v1` in state Draft. The location of every document is its PDF, so the version counts the commits of the PDF.
- **Date:** date of the last commit that changed the file.
- **SHA-256:** the first 16 hexadecimal characters of the file's hash. The full hash comes from `sha256sum <file>`. Planned items show "—".
- **State:** Draft, Under review, Baselined, or Superseded.

# Documents

Table: Document CIs {#tbl-ci-doc}

| ID | Name | Location | Version | Date | SHA-256 | Owner | State | Affected by |
|---|---|---|---|---|---|---|---|---|
| CI-DOC-01 | Baseline v0 | `BaseLine_Windsor_v0.pdf` | v1 | 2026-09-14 | `c92e70e603338f6b` | Project Manager | Superseded | CR-01 |
| CI-DOC-02 | Baseline v1 | `BaseLine_Windsor_v1.pdf` | v1 | 2026-10-02 | `74ba6f58072a750f` | Project Manager | Baselined | CR-03, CR-04 |
| CI-DOC-03 | Original project proposal | `artifacts/windsor-project-proposal-EN.pdf` | v1 | 2026-09-14 | `f87c982692494c75` | Project Manager | Superseded | — |
| CI-DOC-04 | Original SRS | `artifacts/Windsor_SRS_EN.pdf` | v2 | 2026-09-14 | `5e3f97ac14f3a239` | Project Manager | Superseded | — |
| CI-DOC-05 | Risk metrics matrix | `artifacts/risk measurement.pdf` | v1 | 2026-11-19 | `1720875656ba6509` | Senior Developer | Baselined | — |
| CI-DOC-06 | SCMP: change and status accounting | `scmp/Windsor_SCMP_Change_and_Status_Accounting.pdf` | v1 | 2026-11-06 | `6c248181cebb1ca1` | Project Manager | Under review | — |
| CI-DOC-07 | Change analysis CR-01 | `change-analysis/Windsor_CR-01_Change_Analysis.pdf` | v7 | 2026-11-19 | `5e9dc49f89b25559` | Senior Developer | Baselined | CR-01 |
| CI-DOC-08 | Change analysis CR-02 | `change-analysis/Windsor_CR-02_Change_Analysis.pdf` | v2 | 2026-11-19 | `111915129317dda5` | Senior Developer | Superseded | CR-02 (rejected) |
| CI-DOC-09 | Change analysis CR-03 | `change-analysis/Windsor_CR-03_Change_Analysis.pdf` | v2 | 2026-11-19 | `659f50346a725612` | Senior Developer | Under review | CR-03 |
| CI-DOC-10 | Change analysis CR-04 | `change-analysis/Windsor_CR-04_Change_Analysis.pdf` | v2 | 2026-11-19 | `6a13e413a6f77f11` | Senior Developer | Under review | CR-04 |
| CI-DOC-11 | Configuration Item Register (this document) | `scmp/Windsor_CI_Register.pdf` | v2 | 2026-11-19 | — | Project Manager | Draft | — |
| CI-DOC-14 | Consolidated change analysis CR-02 to CR-04 | `change-analysis/Windsor_CR-02-04_Consolidated_Change_Analysis.pdf` | v2 | 2026-11-19 | `91d822cbbabf9227` | Senior Developer | Under review | CR-02, CR-03, CR-04 |
| CI-DOC-15 | Configuration control CR-02 to CR-04 | `configuration-control/Windsor_CR-02-04_Configuration_Control.pdf` | v1 | 2026-11-19 | `4f7054bd815f33c5` | Project Manager | Draft | CR-02, CR-03, CR-04 |

The original proposal and SRS were merged into baseline v0, so they are Superseded. The change analysis of CR-02 is Superseded because the CCB rejected the change (CI-DOC-15); it is archived with the reason for the rejection.

The register cannot hold its own hash. It is checked against the commit that adds it.

# Requirements

Table: Requirement CIs {#tbl-ci-req}

| ID | Name | Location | Version | Date | SHA-256 | Owner | State | Affected by |
|---|---|---|---|---|---|---|---|---|
| CI-REQ-01 | Requirements list | `artifacts/Windsor_Requirements_EN.csv` | v2 | 2026-09-14 | `bd87aae19a5045f6` | Project Manager | Baselined | CR-03, CR-04 |

**Observation:** the requirements list does not match baseline v1:

- it lacks RF-CLI-004, added by CR-01;
- it names RNF-PER-002 where the baseline says RNF-PER-001;
- it says "Provider" where the baseline says "Supplier".

This breaks step 3 of *Delivering a New Baseline* in the SCMP. It is recorded here and not corrected: a correction needs its own change request (policy IC-08). It has to be fixed before baseline v2.

# Figures

Table: Image CIs {#tbl-ci-img}

| ID | Name | Location | Version | Date | SHA-256 | Owner | State | Affected by |
|---|---|---|---|---|---|---|---|---|
| CI-IMG-01 | Architecture (Figure 2) | `images/windsor-architecture.svg` | v1 | 2026-10-02 | `6d2456e7ab06fc02` | Senior Developer | Baselined | — |
| CI-IMG-02 | Architecture of v0 | `images/windsor-architecture.png` | v1 | 2026-09-14 | `8f58670b64c51c88` | Senior Developer | Superseded | — |
| CI-IMG-03 | Deployment communication (Figure 3) | `images/windsor-deployment-communication.svg` | v1 | 2026-10-02 | `2328e799e72a8c44` | Senior Developer | Baselined | — |
| CI-IMG-04 | Module communication (Figure 4) | `images/windsor-module-communication.svg` | v1 | 2026-10-02 | `be018df612279b84` | Senior Developer | Baselined | CR-03 |
| CI-IMG-05 | Sprint schedule (Figure 1) | `images/windsor-schedule.svg` | v1 | 2026-10-02 | `c398082a92d064a0` | Project Manager | Baselined | CR-03, CR-04 |
| CI-IMG-06 | CR-01 deployment v0 | `images/cr-01-deployment-v0.svg` | v1 | 2026-10-09 | `75c171fb0acf856c` | Senior Developer | Baselined | CR-01 |
| CI-IMG-07 | CR-01 deployment v1 | `images/cr-01-deployment-v1.svg` | v1 | 2026-10-09 | `0bd89d550f795dfa` | Senior Developer | Baselined | CR-01 |
| CI-IMG-08 | CR-01 booking flow | `images/cr-01-booking-flow.svg` | v1 | 2026-10-09 | `96561469fbb0af2f` | Senior Developer | Baselined | CR-01 |
| CI-IMG-09 | CR-01 schedule | `images/cr-01-schedule.svg` | v1 | 2026-10-09 | `1c1e3268b61f3830` | Project Manager | Baselined | CR-01 |
| CI-IMG-10 | CR-02 architecture, country view | `images/cr-02-architecture-country.svg` | v1 | 2026-11-12 | `44e93ecc78446758` | Senior Developer | Superseded | CR-02 (rejected) |
| CI-IMG-11 | CR-02 architecture, site view | `images/cr-02-architecture-site.svg` | v1 | 2026-11-12 | `54539d8e91d033eb` | Senior Developer | Superseded | CR-02 (rejected) |
| CI-IMG-12 | CR-02 map flow | `images/cr-02-map-flow.svg` | v1 | 2026-11-12 | `b3100ad1fda31346` | Senior Developer | Superseded | CR-02 (rejected) |
| CI-IMG-13 | CR-02 schedule | `images/cr-02-schedule.svg` | v1 | 2026-11-12 | `a12a68dc73bfd21d` | Project Manager | Superseded | CR-02 (rejected) |
| CI-IMG-14 | CR-03 reporting flow | `images/cr-03-reporting-flow.svg` | v1 | 2026-11-12 | `7b9b6a8562e1995f` | Senior Developer | Draft | CR-03 |
| CI-IMG-15 | CR-03 schedule | `images/cr-03-schedule.svg` | v1 | 2026-11-12 | `c426baa0e9fa9872` | Project Manager | Draft | CR-03 |
| CI-IMG-16 | CR-04 tour flow | `images/cr-04-tour-flow.svg` | v1 | 2026-11-12 | `5da494ee03e8b12e` | Senior Developer | Draft | CR-04 |
| CI-IMG-17 | CR-04 schedule | `images/cr-04-schedule.svg` | v1 | 2026-11-12 | `1bd3934c0bad3525` | Project Manager | Draft | CR-04 |

CI-IMG-10 to CI-IMG-13 belong to the rejected CR-02 and are archived with it. CI-IMG-01 and CI-IMG-03 stay as the architecture and deployment figures of baseline v2.

# Planned Items

These items (baseline v2, the design system, code, databases, and configuration) are defined by the baseline and the approved change requests. They do not exist yet. Each one gets a version and a hash when it is created: baseline v2 when it is published, and the rest when their first pull request is merged.

Table: Planned CIs {#tbl-ci-planned}

| ID | Name | Location | Version | Date | SHA-256 | Owner | State | Affected by |
|---|---|---|---|---|---|---|---|---|
| CI-DOC-12 | Baseline v2 | `BaseLine_Windsor_v2.pdf` | — | — | — | Project Manager | Draft | CR-03, CR-04 |
| CI-DOC-13 | Client screen design system | Figma (supplier) | — | — | — | Project Manager | Draft | CR-04 |
| CI-CODE-01 | Windsor monolith (NestJS and Angular) | Application repository | — | — | — | Senior Developer | Draft | CR-03 (AML module and API), CR-04 (redesign and 360° viewer) |
| CI-DB-01 | Database schema | Application repository (migrations) | — | — | — | Mid-level Developer | Draft | CR-03 |
| CI-CFG-01 | Nginx proxy configuration | Vultr VPS | — | — | — | Senior Developer | Draft | CR-01, CR-03 |
| CI-CFG-02 | WireGuard tunnel configuration | Vultr VPS and the Monterrey LAN | — | — | — | Senior Developer | Draft | CR-01 |
| CI-CFG-06 | AML module contract and settings | KYC Systems (supplier) | — | — | — | Project Manager | Draft | CR-03 |
| CI-CFG-07 | Object Storage bucket and retention rules | Vultr (supplier) | — | — | — | Senior Developer | Draft | CR-03, CR-04 |

The Nginx and WireGuard configurations were introduced by CR-01. They are listed only now because the SCMP's register did not exist when CR-01 was released.

The items that only CR-02 planned do not enter the configuration, because the CCB rejected that change (CI-DOC-15): CI-CODE-02 to CI-CODE-05, CI-DB-02, CI-CFG-03 to CI-CFG-05, CI-CFG-08, and CI-CFG-09. Their IDs are not reused.

# Release per Environment

With CR-02 rejected, Windsor runs at one venue, Monterrey. Status accounting records which release runs there. The Senior Developer updates this table the same day as each installation (policy SA-03).

Table: Release installed at each environment {#tbl-ci-sites}

| Environment | Release | Installation date | Responsible |
|---|---|---|---|
| QA | Not installed | — | Senior Developer |
| Monterrey production | Not installed (1.0.0 planned at week 14) | — | Senior Developer |

# Items Not Under Configuration Control

The essay in `homework/` and the repository readme are not part of the Windsor system, so they are not configuration items.
