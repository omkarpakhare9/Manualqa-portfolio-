Defect Log: Apex Digital Banking Portal

This document logs defects identified during functional, boundary, and concurrency testing on build `v1.4.0-rc2`.



[BUG-001] Rapid consecutive clicks on "Confirm Transfer" triggers duplicate debit transactions

* Defect ID: `BUG-001`
* Linked Test Case: `TC-FT-007`
* Severity: S1 (Blocker / Financial Discrepancy)
* Priority: P1 (High)
* Component: Payments / Transaction Processing API
* Environment: Staging `v1.4.0-rc2` (Chrome 128 / Android 14 & macOS Sonoma)
* Status: Open
* Reported Date: 2026-10-02

Summary
The client-side submission handler does not disable or debounce the "Confirm Transfer" button upon user tap. When a user double-clicks or rapidly taps the button twice within ~200 milliseconds, two parallel API requests (`POST /api/v1/transfers/process`) are dispatched with the same payload. The backend lacks an idempotency key mechanism, leading to duplicate account debits and two distinct Unique Reference Numbers (URNs) generated for a single intended payment.

Steps to Reproduce
1. Log in to the retail banking portal using valid credentials (`CustID: 80921102`).
2. Navigate to Transfers > Quick IMPS Transfer.
3. Select Source Account: `Savings A/c ****4401` (Available Balance: ₹1,50,000).
4. Select Beneficiary: `Ben-01 (HDFC - ****8921)`.
5. Enter Amount: `₹2,500.00`.
6. Enter valid 2FA SMS OTP and reach the final confirmation modal.
7. Rapidly double-tap the "Confirm Transfer" button (interval < 250ms).

Expected Result
* The "Confirm Transfer" button must immediately transition to a disabled state with a loading spinner on the first tap.
* Only one `POST` request should be sent to `/api/v1/transfers/process`.
* The source account should be debited exactly once (`₹2,500.00`).
* One URN should be generated, and the user redirected to a single receipt screen.

Actual Result
* The UI button accepts multiple clicks without immediate visual lockout.
* Two concurrent HTTP requests are sent to the payment gateway endpoint.
* Both requests succeed: The account is debited twice (`₹2,500.00 x 2 = ₹5,000.00`).
* Two distinct confirmation prompts and two separate URNs are logged in the transaction ledger:
  * Transaction 1: `URN-APX-20261002-88219` (₹2,500.00 Debited)
  * Transaction 2: `URN-APX-20261002-88220` (₹2,500.00 Debited)

Business & Financial Impact
High financial and legal risk. Customers risk unauthorized double deductions from their available balance, leading to failed subsequent standing instructions, customer distress, and manual settlement overhead.

Suggested Fix
1. Frontend: Implement immediate button disablement and state lock on first submission event.
2. Backend: Enforce idempotency using a unique `Idempotency-Key` header generated on transaction modal initiation. Reject duplicate transaction tokens within a 5-minute rolling window.



[BUG-002] Negative transfer values accepted when submitted via browser input bypass

* Defect ID: `BUG-002`
* Linked Test Case: `TC-FT-003`
* Severity: S2 (critical)
* Priority: P2 (Medium-High)
* Component: Transfer Form Validation
* Environment: Staging `v1.4.0-rc2`
* Status: Open

Summary
While the UI restricts negative signs in standard key entry, pasting negative values (e.g., `-500.00`) directly via clipboard bypasses client validation and sends an unhandled HTTP 500 error instead of a clean field validation message.

Steps to Reproduce
1. Navigate to Fund Transfers.
2. Copy the text `-500.00` to the clipboard.
3. Paste the value directly into the Amount (₹) input field.
4. Tap Continue.

Expected Result
Input field rejects pasted negative numeric values and displays: "Amount must be a positive number between ₹1.00 and ₹2,00,000.00".

Actual Result
Field retains `-500.00`. Upon tapping Continue, the page displays a generic uncaught error modal: "Internal Application Error (HTTP 500)" and logs an unhandled `NumberFormatException` in the console.
