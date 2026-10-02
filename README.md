# Manualqa-portfolio-
End to End manual qa documentation 
Apex Digital Banking Portal — Comprehensive Manual QA Portfolio
![Status](https://img.shields.io/badge/Release-v1.4.0--rc2-blue)
![Testing Type](https://img.shields.io/badge/Testing-Manual%20%7C%20Functional%20%7C%20Security-green)
![Coverage](https://img.shields.io/badge/Requirements%20Covered-100%25-brightgreen) 
Project Overview
This repository contains a production-grade manual quality assurance audit conducted on the Apex Digital Banking Portal (Retail Web Platform). The testing cycle validates core retail banking workflows with a focus on financial integrity, boundary value constraints, transaction idempotency, and session security.

Test Execution Summary

| Metric | Count | Percentage |

| Total Test Scenarios Executed | 10 | 100% |
| Passed Scenarios | 8 | 80% |
| Failed Scenarios (Defects Logged) | 2 | 20% |
| Critical / Blocker Bugs Identified | 1 (S1 Blocker) | 10% |
| Requirements Traceability Coverage | 8 / 8 | 100% |


Repository Structure & Artifacts

| Document | Description | Direct Link |

| Test Plan & Strategy | Scope, test environments, entry/exit criteria, and test methodologies | [View Test Plan](docs/TEST_PLAN.md) |
| Test Case Suite | Detailed functional, BVA, and security edge test cases for Fund Transfers | [View Test Cases](test-cases/fund-transfers.md) |
| Defect Log | Comprehensive bug tickets with reproduction steps, actual vs. expected, and fix proposals | [View Defect Reports](docs/BUG_REPORTS.md) |
|Requirements Traceability Matrix (RTM)| Direct mapping between business specs, test cases, and discovered defects | [View RTM](docs/RTM.md) |


Testing Methodologies & Techniques Applied
* Boundary Value Analysis (BVA) & Equivalence Partitioning (EP): Validated transfer amount thresholds (₹1 min, ₹2,00,000 daily cap, decimal precision limits).
* Concurrency & Race Condition Verification: Tested button debouncing and transaction idempotency during rapid double-tap events on payment triggers.
* Negative & Exception Testing: Form handling for negative balances, clipboard paste bypasses, and account lockout after consecutive invalid 2FA OTPs.
* Session Integrity & State Handling: Browser back-button navigation following transaction completion to prevent duplicate postbacks.

Key Findings Highlight
* Critical Finding (`BUG-001` - S1 Blocker): Missing idempotency guard on the "Confirm Transfer" action allows concurrent API requests, causing duplicate fund debits for a single transaction.
* Input Validation Finding (`BUG-002` - S2 Critical): Direct paste of negative numeric values into the transfer amount field causes an unhandled HTTP 500 server exception.
