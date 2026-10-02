Test Cases: Fund Transfers & Transaction Limits
Module: Payments & Transfers  
Application: Apex Digital Banking Portal  
Test Suite Type: Functional, Boundary Value Analysis (BVA), Negative & Security Edge Cases  

1. Pre-requisites & Test Accounts
* Source Account A (Savings): Balance = ₹1,50,000 | Daily Transfer Limit Remaining = ₹2,00,000
* Source Account B (Low Balance): Balance = ₹500 | Daily Transfer Limit Remaining = ₹50,000
* Source Account C (Max Limit Reached): Balance = ₹3,00,000 | Daily Transfer Limit Remaining = ₹0
* Beneficiary Ben-01: Registered & Activated (IMPS/NEFT eligible)
* Beneficiary Ben-02: Cooling-off period active (Max allowed per transfer = ₹10,000)

2. Test Execution Matrix

| Test ID | Module / Feature | Scenario / Objective | Test Type | Pre-conditions | Test Steps | Test Data | Expected Result | Status |

| TC-FT-001 | Quick IMPS Transfer | Verify successful fund transfer within single transaction limit | Positive | Account A active; Ben-01 active | 1. Select Source Account A<br>2. Select Ben-01<br>3. Enter valid amount<br>4. Submit & enter valid OTP | Amount: ₹5,000<br>OTP: `482910` | 1. Status: "Transaction Successful"<br>2. Unique Reference Number (URN) generated<br>3. Balance deducted to ₹1,45,000 | PASS |
| TC-FT-002 | Transfer Limits | Minimum boundary transfer validation (₹1) | BVA (Boundary) | Account A active | 1. Select Source Account A<br>2. Select Ben-01<br>3. Enter minimum allowed value (₹1)<br>4. Confirm transfer | Amount: `₹1.00` | Transaction succeeds without decimal truncation; debit reflects exact amount | PASS |
| TC-FT-003 | Transfer Limits | Transfer amount lower than minimum boundary (₹0 or negative) | Negative / BVA | Account A active | 1. Navigate to Transfer form<br>2. Enter ₹0.00 or -₹500<br>3. Attempt to submit | Amount: `0` or `-500` | "Initiate Transfer" button remains disabled; inline validation: "Amount must be between ₹1 and ₹2,00,000" | PASS |
| TC-FT-004 | Balance Integrity | Transfer amount greater than available balance | Negative | Account B active (Balance ₹500) | 1. Select Source Account B<br>2. Enter amount exceeding balance<br>3. Tap submit | Amount: `₹501.00` | Immediate form rejection before OTP prompt: "Insufficient funds in selected account" | PASS |
| TC-FT-005 | Daily Limit Gate | Transfer attempt when daily limit is exhausted | Negative / Business Rule | Account C active (Limit ₹0 remaining) | 1. Select Source Account C<br>2. Enter valid transfer amount<br>3. Submit | Amount: `₹1,000.00` | Error banner: "Daily transfer limit of ₹2,00,000 reached. Please try tomorrow or modify limit in settings." | PASS |
| TC-FT-006 | Beneficiary Rules | Enforce cooling-off restriction on newly added payee | Business Logic | Beneficiary Ben-02 added within 2 hours | 1. Select Ben-02<br>2. Enter amount exceeding cooling cap (₹10,001)<br>3. Submit | Amount: `₹15,000` | Warning prompt: "Beneficiary under cooling period. Max allowed: ₹10,000. Available limit resets in 2 hours." | PASS |
| TC-FT-007 | Double-Click Guard | Rapid duplicate clicks on final "Pay Now" button | Concurrency / Idempotency | Account A active | 1. Fill valid transfer details<br>2. Rapidly double-click "Submit Transfer" within 200ms | Amount: `₹2,500` | UI disables button on first click with spinner; only one debit occurs (single URN issued) | FAIL |
| TC-FT-008 | 2FA Security | Transfer rejection upon 3 consecutive invalid OTP inputs | Security / Negative | Valid transfer initiated | 1. Trigger OTP to mobile<br>2. Enter incorrect OTP 3 times consecutively | OTP: `000000`, `111111`, `222222` | Transaction aborted; transfer session terminated; OTP invalidated; security alert sent via SMS | PASS |
| TC-FT-009 | Decimal Precision | Boundary test on decimal amounts (>2 decimal places) | Boundary / Input Sanity | Account A active | 1. Input amount with 3 decimals (`100.555`) | Amount: `100.555` | Field auto-restricts to 2 decimals or blocks extra keystroke (`100.55`) | PASS |
| TC-FT-010 | Session Security | Browser back button navigation post-transaction completion | Security / Session State | `TC-FT-001` completed | 1. Complete transfer<br>2. Land on Receipt screen<br>3. Tap Browser Back button | Navigation action | Redirects to Dashboard or shows "Form Resubmission Prevented"; no duplicate payment prompt | PASS |
