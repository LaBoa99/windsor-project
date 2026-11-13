---
title: Configuration Item Register
subtitle: "SCMP form F-04 for the “Windsor” Integrated Management System, with the items added by CR-02, CR-03, and CR-04"
running_title: SCMP — Configuration Item Register — Windsor
author: General México Software Co.
date: 2026-11-06
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
- **Version:** `v{n}`, where n is the number of commits that changed the file, so that anyone can check it with `git log --follow`. Items not committed yet are `v1` in state Draft.
- **Date:** date of the last commit that changed the file, or 2026-11-06 for new items.
- **SHA-256:** the first 16 hexadecimal characters of the file's hash. The full hash comes from `sha256sum <file>`. Planned items show "—".
- **State:** Draft, Under review, Baselined, or Superseded.

# Documents

Table: Document CIs {#tbl-ci-doc}

| ID | Name | Location | Version | Date | SHA-256 | Owner | State | Affected by |
|---|---|---|---|---|---|---|---|---|
| CI-DOC-01 | Baseline v0 | `BaseLine_Windsor_v0.pdf` | v1 | 2026-09-14 | `c92e70e603338f6b` | Project Manager | Superseded | CR-01 |
| CI-DOC-02 | Baseline v1 | `BaseLine_Windsor_v1.pdf` | v1 | 2026-10-02 | `74ba6f58072a750f` | Project Manager | Baselined | CR-02, CR-03, CR-04 |
| CI-DOC-03 | Original project proposal | `artifacts/windsor-project-proposal-EN.pdf` | v1 | 2026-09-14 | `f87c982692494c75` | Project Manager | Superseded | — |
| CI-DOC-04 | Original SRS | `artifacts/Windsor_SRS_EN.pdf` | v2 | 2026-09-14 | `5e3f97ac14f3a239` | Project Manager | Superseded | — |
| CI-DOC-05 | Risk metrics matrix | `artifacts/risk measurement.md` | v5 | 2026-10-16 | `a67fd9b9b85d32eb` | Senior Developer | Baselined | — |
| CI-DOC-06 | SCMP: change and status accounting | `scmp/Windsor_SCMP_Change_and_Status_Accounting.md` | v1 | 2026-11-06 | `4162a1dc75880b42` | Project Manager | Under review | — |
| CI-DOC-07 | Change analysis CR-01 | `change-analysis/Windsor_CR-01_Change_Analysis.md` | v4 | 2026-10-16 | `2c90dcc6e46edbd5` | Senior Developer | Baselined | CR-01 |
| CI-DOC-08 | Change analysis CR-02 | `change-analysis/Windsor_CR-02_Change_Analysis.md` | v1 | 2026-11-06 | `c2a034e359128c1a` | Senior Developer | Draft | CR-02 |
| CI-DOC-09 | Change analysis CR-03 | `change-analysis/Windsor_CR-03_Change_Analysis.md` | v1 | 2026-11-06 | `43511783f5968799` | Senior Developer | Draft | CR-03 |
| CI-DOC-10 | Change analysis CR-04 | `change-analysis/Windsor_CR-04_Change_Analysis.md` | v1 | 2026-11-06 | `6db7f979e62fbbee` | Senior Developer | Draft | CR-04 |
| CI-DOC-11 | Configuration Item Register (this document) | `scmp/Windsor_CI_Register.md` | v1 | 2026-11-06 | — | Project Manager | Draft | — |

The original proposal and SRS were merged into baseline v0, so they are Superseded. The rendered PDFs of the Markdown documents are outputs of their sources, not separate items. The source of baseline v1 is not in the repository, so the PDF is the item.

The register cannot hold its own hash. It is checked against the commit that adds it.

# Requirements

Table: Requirement CIs {#tbl-ci-req}

| ID | Name | Location | Version | Date | SHA-256 | Owner | State | Affected by |
|---|---|---|---|---|---|---|---|---|
| CI-REQ-01 | Requirements list | `artifacts/Windsor_Requirements_EN.csv` | v2 | 2026-09-14 | `bd87aae19a5045f6` | Project Manager | Baselined | CR-02, CR-03, CR-04 |

**Observation:** the requirements list does not match baseline v1:

- it lacks RF-CLI-004, added by CR-01;
- it names RNF-PER-002 where the baseline says RNF-PER-001;
- it says "Provider" where the baseline says "Supplier".

This breaks step 3 of *Delivering a New Baseline* in the SCMP. It is recorded here and not corrected: a correction needs its own change request (policy IC-08). It has to be fixed before baseline v2.

# Figures

Table: Image CIs {#tbl-ci-img}

| ID | Name | Location | Version | Date | SHA-256 | Owner | State | Affected by |
|---|---|---|---|---|---|---|---|---|
| CI-IMG-01 | Architecture (Figure 2) | `images/windsor-architecture.svg` | v1 | 2026-10-02 | `6d2456e7ab06fc02` | Senior Developer | Baselined | CR-02 |
| CI-IMG-02 | Architecture of v0 | `images/windsor-architecture.png` | v1 | 2026-09-14 | `8f58670b64c51c88` | Senior Developer | Superseded | — |
| CI-IMG-03 | Deployment communication (Figure 3) | `images/windsor-deployment-communication.svg` | v1 | 2026-10-02 | `2328e799e72a8c44` | Senior Developer | Baselined | CR-02 |
| CI-IMG-04 | Module communication (Figure 4) | `images/windsor-module-communication.svg` | v1 | 2026-10-02 | `be018df612279b84` | Senior Developer | Baselined | CR-02, CR-03 |
| CI-IMG-05 | Sprint schedule (Figure 1) | `images/windsor-schedule.svg` | v1 | 2026-10-02 | `c398082a92d064a0` | Project Manager | Baselined | CR-02, CR-03, CR-04 |
| CI-IMG-06 | CR-01 deployment v0 | `images/cr-01-deployment-v0.svg` | v1 | 2026-10-09 | `75c171fb0acf856c` | Senior Developer | Baselined | CR-01 |
| CI-IMG-07 | CR-01 deployment v1 | `images/cr-01-deployment-v1.svg` | v1 | 2026-10-09 | `0bd89d550f795dfa` | Senior Developer | Baselined | CR-01 |
| CI-IMG-08 | CR-01 booking flow | `images/cr-01-booking-flow.svg` | v1 | 2026-10-09 | `96561469fbb0af2f` | Senior Developer | Baselined | CR-01 |
| CI-IMG-09 | CR-01 schedule | `images/cr-01-schedule.svg` | v1 | 2026-10-09 | `1c1e3268b61f3830` | Project Manager | Baselined | CR-01 |
| CI-IMG-10 | CR-02 architecture, country view | `images/cr-02-architecture-country.svg` | v1 | 2026-11-06 | `44e93ecc78446758` | Senior Developer | Draft | CR-02 |
| CI-IMG-11 | CR-02 architecture, site view | `images/cr-02-architecture-site.svg` | v1 | 2026-11-06 | `54539d8e91d033eb` | Senior Developer | Draft | CR-02 |
| CI-IMG-12 | CR-02 map flow | `images/cr-02-map-flow.svg` | v1 | 2026-11-06 | `b3100ad1fda31346` | Senior Developer | Draft | CR-02 |
| CI-IMG-13 | CR-02 schedule | `images/cr-02-schedule.svg` | v1 | 2026-11-06 | `a12a68dc73bfd21d` | Project Manager | Draft | CR-02 |
| CI-IMG-14 | CR-03 reporting flow | `images/cr-03-reporting-flow.svg` | v1 | 2026-11-06 | `7b9b6a8562e1995f` | Senior Developer | Draft | CR-03 |
| CI-IMG-15 | CR-03 schedule | `images/cr-03-schedule.svg` | v1 | 2026-11-06 | `c426baa0e9fa9872` | Project Manager | Draft | CR-03 |
| CI-IMG-16 | CR-04 tour flow | `images/cr-04-tour-flow.svg` | v1 | 2026-11-06 | `5da494ee03e8b12e` | Senior Developer | Draft | CR-04 |
| CI-IMG-17 | CR-04 schedule | `images/cr-04-schedule.svg` | v1 | 2026-11-06 | `1bd3934c0bad3525` | Project Manager | Draft | CR-04 |

When baseline v2 is published, CI-IMG-10 and CI-IMG-11 replace CI-IMG-01 and CI-IMG-03 as the architecture and deployment figures. CI-IMG-01 and CI-IMG-03 then become Superseded.

# Code, Databases, and Configuration (planned)

These items are defined by the baseline and the change requests. They do not exist in this repository yet. Each one gets a version and a hash when its first pull request is merged.

Table: Planned CIs {#tbl-ci-planned}

| ID | Name | Location | Version | Date | SHA-256 | Owner | State | Affected by |
|---|---|---|---|---|---|---|---|---|
| CI-CODE-01 | Windsor monolith (NestJS and Angular) | Application repository | — | — | — | Senior Developer | Draft | CR-03 (AML module and API), CR-02 (split) |
| CI-CODE-02 | Client Service image | Docker Hub, private repository | — | — | — | Senior Developer | Draft | CR-02, CR-03, CR-04 |
| CI-CODE-03 | Hall Administration image | Docker Hub, private repository | — | — | — | Senior Developer | Draft | CR-02, CR-04 |
| CI-CODE-04 | Sync Service image | Docker Hub, private repository | — | — | — | Senior Developer | Draft | CR-02 |
| CI-CODE-05 | Site node image | Docker Hub, private repository | — | — | — | Senior Developer | Draft | CR-02 |
| CI-DB-01 | Site database schema | Application repository (migrations) | — | — | — | Mid-level Developer | Draft | CR-02, CR-03 |
| CI-DB-02 | Cloud database schema | Application repository (migrations) | — | — | — | Mid-level Developer | Draft | CR-02 |
| CI-CFG-01 | Nginx proxy configuration | Vultr VPS | — | — | — | Senior Developer | Draft | CR-01, CR-02, CR-03 |
| CI-CFG-02 | WireGuard tunnel configuration | Vultr VPS and each site | — | — | — | Senior Developer | Draft | CR-01, CR-02 |
| CI-CFG-03 | Site configuration: Monterrey | Monterrey site node | — | — | — | Senior Developer | Draft | CR-02 |
| CI-CFG-04 | Site configuration: beach venue | Beach site node | — | — | — | Senior Developer | Draft | CR-02 |
| CI-CFG-05 | Site configuration: mountain venue | Mountain site node | — | — | — | Senior Developer | Draft | CR-02 |
| CI-CFG-06 | AML module contract and settings | KYC Systems (supplier) | — | — | — | Project Manager | Draft | CR-03 |
| CI-CFG-07 | Object Storage bucket and retention rules | Vultr (supplier) | — | — | — | Senior Developer | Draft | CR-03, CR-02, CR-04 |
| CI-CFG-08 | Docker Hub repositories and access | Docker Hub (supplier) | — | — | — | Senior Developer | Draft | CR-02 |
| CI-CFG-09 | CI pipeline | Application repository (GitHub Actions) | — | — | — | Senior Developer | Draft | CR-02 |

The Nginx and WireGuard configurations were introduced by CR-01. They are listed only now because the SCMP's register did not exist when CR-01 was released.

Each new venue adds one more site configuration (CI-CFG-nn). The image is the same for every venue, so only the configuration changes.

# Version per Site

CR-02 makes every venue run its own copy of the Site node. Status accounting therefore has to say which release runs where. The Senior Developer updates this table the same day as each installation (policy SA-03).

Table: Release installed at each site {#tbl-ci-sites}

| Site | Configuration | Site node release | Installation date | Responsible |
|---|---|---|---|---|
| Monterrey | CI-CFG-03 | Not installed (1.0.0 planned at week 14) | — | Senior Developer |
| Beach venue | CI-CFG-04 | Not installed (opens after the MVP) | — | Senior Developer |
| Mountain venue | CI-CFG-05 | Not installed (opens after the MVP) | — | Senior Developer |

A site that runs a release different from the others is a version drift (risk in CR-02). The difference is recorded as a deviation (DEV-nn) until the site is updated.

# Items Not Under Configuration Control

`homework/essay homework scm.pdf` and `README.md` belong to the course work, not to the Windsor system, so they are not configuration items.
