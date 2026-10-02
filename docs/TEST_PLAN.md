Test Plan: Apex Digital Banking Portal (Retail Web)

1. Overview & Objective
Apex Digital Bank is a secure web-based retail banking application. The objective of this testing cycle is to conduct comprehensive functional, boundary, negative, and security compliance verification on the retail banking module prior to release v1.4.0.

2. Scope of Testing

In-Scope
Customer Authentication & Security:
  * Login with Customer ID/Password and SMS/Email OTP (2FA)
  * Session timeout after inactivity
  * Password masking, virtual keypad support, and brute-force lockout
  Fund Transfers:
  * Self-Account Transfer, Third-Party Internal Transfer, and External Transfers (IMPS/NEFT)
  * Daily transaction limits and balance insufficiency checks
  * OTP confirmation for beneficiary addition and high-value transfers
  Account Dashboard & Statements:
  * Real-time balance updates post-debit/credit
  * Transaction history filtering (date range, transaction type, amount)
  * Statement download (PDF and CSV formats)

Out-of-Scope
* Core banking mainframe batch processing runs
* Performance and load testing under concurrent 10,000+ users (handled by performance engineering team)
* Corporate/Commercial multi-signatory authorization workflows

3. Test Environment & Matrix
Application URL: `https://staging-retail.apexbank.internal`
Test Accounts:
  * Account A (Savings - Active, Balance: ₹1,50,000)
  * Account B (Current - Active, Balance: ₹25,000)
  * Account C (Dormant / Inactive Account)
  * Account D (Savings - Low Balance: ₹500, Daily Limit Reached)
  Platforms Tested:
  * Desktop: Chrome v128+, Firefox v129+, Edge v128+
  * Mobile Browsers: Chrome on Android 14, Safari on iOS 17

4. Test Strategy & Methodologies
* Equivalence Partitioning (EP): Transfer amount tiers, account numbers, OTP lengths.
* Boundary Value Analysis (BVA): Min/max transfer limits (e.g., ₹1, ₹49,999, ₹50,000, ₹2,00,000).
* Negative & Exception Testing: Expired OTPs, SQL injection payloads in input fields, rapid double-click on "Confirm Transfer".
* Session & State Integrity: Browser back button after logout, multi-tab transactions.

5. Defect Severity & Priority Classification
* S1 (Blocker): Financial discrepancy (wrong balance deducted/credited), security vulnerability, or unhandled 500 server crash during transfer.
* S2 (Critical): Transfer fails without error message, OTP fails to generate, or transaction history fails to load.
* S3 (Major): Statement download formatting broken, incorrect validation tooltip, or search filter sorting error.
* S4 (Minor): Typographical error, alignment issue, or minor UI cosmetic defect.

6. Entry & Exit Criteria
* Entry Criteria: Staging build deployed, smoke test 100% passed, mock accounts seeded with known balances and test beneficiaries.
* Exit Criteria: 100% of planned test cases executed, 0 open S1/S2 defects, all transaction audit logs verified against mock bank ledger.
  
