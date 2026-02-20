# OED Penetration Test – Status Tracking

## Purpose
This document tracks findings from the Open Energy Dashboard (OED)
penetration testing report and maps them to current GitHub issues,
pull requests, and implementation status.

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

## Summary
- Total Findings: 19  
- ✅ Done: 1  
- 🟡 In Progress: 12 
- 🔴 Not Started: 3  
- ⚠️ Design Only: 11

Note: “Upstream OED PR Evidence” refers to pull requests in the
official OpenEnergyDashboard/OED repository when applicable.

## Penetration Test Findings

| Problem # | Title | Severity | Team-5 Issue(s) | Upstream OED PR Evidence | Current Status | Owner | Notes / Remaining Work |
|-----------|-------|----------|------------------|---------------------------|----------------|--------|-------------------------|
| 1 | Insufficient Access Controls | Critical | Issue #118 | — | 🟡 In Progress | Andrew | Access control enforcement still incomplete |
| 2 | Insecure Default Configuration | High | Team-5 tracking | OED PR #1554 | 🟡 In Progress | Zack | Fix exists but PR still open and requires changes |
| 3 | Cross-Site Scripting (XSS) | Medium | #69, #98 | OED PR #1544 | ✅ Done | Zach | Fix and validation completed |
| 4 | Insecure Docker Configuration | Medium | #70, #99 | — | 🔴 Not Started | — | No assigned owner and no implementation work initiated |
| 5 | Hard-Coded Database Credentials | Medium | #71, #101 | — | 🟡 Research In Progress
 | Oye | Research document uploaded; secure secret-management design phase pending implementation |
| 6 | Missing Content Security Policy | Medium | #72, #102 | OED PR #1567 | 🟡 In Progress | Brian | PR open and requires requested changes |
| 7 | Known Vulnerabilities in Software Components | Medium | Design docs | — | 🟡 In Progress | Oye, Andrew | Design approved; supplemental security implementation in progress|
| 8 | Insufficient Input Validation | Medium | #74, #106 | — | 🟡 In Progress | Brian | Validation testing still ongoing |
| 9 | File Upload Denial of Service | Medium | #75, #88 | — | 🟡 In Progress | Krista | Upload protections not finalized |
| 10 | Insecure Password Authentication | Minimal | #76, #108 | — | 🔴 Not Started | — | No assigned owner or merged fix |
| 11 | Insufficient Session Expiration | Low | Design PR #144 | — | ⚠️ Design Only | Oye | Design complete; implementation deferred |
| 12 | Insufficient Brute Force Protection | Low | #78, PR #143 | — | 🟡 In Progress | Zach | Fix merged; validation still open |
| 13 | Valid User Enumeration | Minimal | #79, #111 | — | 🟡 In Progress | Andrew | Implementation not yet merged |
| 14 | Information Disclosure | Minimal | #80, #93 | — | 🟡 In Progress | Zack | Design and validation pending |
| 15 | Clickjacking (UI Redress) | Low | #81, #113 | — | 🔴 Not Started | — | No assigned owner and no implementation work initiated |
| 16 | Log Injection | Minimal | #82, #114 | — | 🟡 In Progress | Zach | Sanitization incomplete |
| 17 | Session Tokens Stored in Local Storage | Low | #83, #115 | — | 🟡 Implementation In Progress| Krista | Migrating authentication from local storage to secure cookies; validation pending |
| 18 | Incorrect HTTP Response Codes | Minimal | #84, #116 | — | 🟡 Design & Implementation In Progress  | Andrew | Remediation strategy and implementation underway |
| 19 | Business Logic Issues | Minimal | #85, #117 | — | 🟡 Design & Validation In Progress | Brian | Business logic review and validation testing ongoing |
