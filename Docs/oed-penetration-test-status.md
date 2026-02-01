# OED Penetration Test – Status Tracking

## Purpose
This document tracks the findings from the Open Energy Dashboard (OED)
penetration testing report and maps them to current GitHub work.

The goal is to:
- Show what has already been addressed
- Identify what is currently being worked on
- Highlight what still needs attention
- Help the team choose the next tasks efficiently

## Source
Open Energy Dashboard – Penetration Test Final Report (July 25, 2024)

## Status Legend
- ✅ **Done** — fix merged and reasonably validated
- 🟡 **In Progress** — issue and/or PR exists, work ongoing
- 🔴 **Not Started** — no implementation work yet
- ⚠️ **Design / Discussion Only** — discussed but not implemented

## Summary (to be updated)
- Total Findings:
- ✅ Done:
- 🟡 In Progress:
- 🔴 Not Started:
- ⚠️ Design Only:

## Penetration Test Findings

| Problem # | Title | Severity | Related GitHub Issue(s) | Current Status | Notes / Remaining Work | Priority |
|----------|-------|----------|--------------------------|---------------|------------------------|----------|
| 1 | Insufficient Access Controls | Critical | Issue #118 (and related access-control discussions) | 🟡 In Progress | Routes documented; access control enforcement and validation still incomplete  | P1 |
| 2 | Insecure Default Configuration | High | Dependency update PR: #139 (dotenv upgrade) (supporting, not a fix)  | ⚠️ Design | Secrets moved to environment variables, but missing validation and silent failure if .env is absent allows insecure defaults |  P1 |
| 3 | Cross-Site Scripting (XSS) | Medium | Issue #69 (fix) + Issue #98 (test/validation) | ✅ Done | Fix and validation completed and issues closed in GitHub. | P2 |
| 4 | Insecure Docker Configuration | Medium |  #70, #99, #29 |  🟡 In Progress | Some Docker hardening work completed (separate Dockerfile), but key security issues (root user, writable files, resource limits, secrets handling) remain open and require fixes and validation  | P1 |
| 5 | Hard-Coded Database Credentials | Medium | #71, #30, #101 | 🟡 In Progress | Tracking, review/design, and test/validation issues exist. Need to confirm removal of all hard-coded credentials, validate DB user documentation, and verify secure configuration via testing. | P2 |
| 6 | Missing Content Security Policy | Medium | Issues #72, #102, #31 | ✅ Done | CSP implemented and validated; design, implementation, and testing completed | P2 |
| 7 | Known Vulnerabilities in Software Components | Medium | Design docs in repo; related dependency discussions | ⚠️ Design / Discussion Only | Vulnerable components identified and documented, but no full implementation or validation completed | P2 |
| 8 | Insufficient Input Validation |  |  |  |  |  |
| 9 | File Upload Denial of Service |  |  |  |  |  |
|10 | Insecure Password Authentication |  |  |  |  |  |
|11 | Insufficient Session Expiration |  |  |  |  |  |
|12 | Insufficient Brute Force Protection |  |  |  |  |  |
|13 | Valid User Enumeration |  |  |  |  |  |
|14 | Information Disclosure |  |  |  |  |  |
|15 | Clickjacking (UI Redress) |  |  |  |  |  |
|16 | Log Injection |  |  |  |  |  |
|17 | Session Tokens Stored in Local Storage |  |  |  |  |  |
|18 | Incorrect / Inconsistent HTTP Response Codes |  |  |  |  |  |
|19 | Business Logic Issues |  |  |  |  |  |
