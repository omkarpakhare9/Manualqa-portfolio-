Requirements Traceability Matrix (RTM)
Project: Apex Digital Banking Portal (Retail Web)  
Release: v1.4.0-rc2  
Module: Customer Authentication, Security & Fund Transfers  

1. Traceability Summary
* Total Business Requirements Tracked: 8
* Requirements with Test Coverage: 8 (100%)
* Requirements Passing Validation: 6
* Requirements with Identified Defects: 2 (REQ-FT-05, REQ-FT-02)

2. Traceability Matrix

| Requirement ID | Requirement Description | Test Case ID | Test Category | Execution Status | Defect ID | Defect Severity |

| `REQ-FT-01` | Allow standard IMPS transfers to verified payees up to daily single limit | `TC-FT-001` | Functional / Positive | PASS | — | — |
| `REQ-FT-02` | Validate minimum transaction threshold of ₹1.00 and reject zero or negative amounts | `TC-FT-002`<br>`TC-FT-003` | Boundary / BVA | PARTIAL | `BUG-002` | S2 (Critical) |
| `REQ-FT-03` | Prevent fund debit when transfer amount exceeds available account balance | `TC-FT-004` | Negative / Business Rule | PASS | — | — |
| `REQ-FT-04` | Reject transfers when the cumulative 24-hour retail cap (₹2,00,000) is exceeded | `TC-FT-005` | Boundary / Business Logic | PASS | — | — |
| `REQ-FT-05` | Prevent duplicate debits on concurrent or rapid double-tap submission | `TC-FT-007` | Concurrency / Idempotency | FAIL | `BUG-001` | S1 (Blocker) |
| `REQ-FT-06` | Enforce statutory cooling-off transfer limits (₹10,000) on newly registered payees | `TC-FT-006` | Compliance / Security | PASS | — | — |
| `REQ-FT-07` | Terminate session and invalidate transaction tokens after 3 consecutive bad OTPs | `TC-FT-008` | Security / 2FA | PASS | — | — |
| `REQ-FT-08` | Block page resubmission and prevent re-triggering transactions via browser navigation | `TC-FT-010` | Session State / Navigation | PASS | — | — |
