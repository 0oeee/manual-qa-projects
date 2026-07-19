# 02 · Banking Application Testing

**In one sentence:** testing money-transfer flows in a training banking app, cross-checking every balance shown on screen against the actual database value.

## Scope

• Transfers between own accounts and to third parties<br>
• Balance calculation and display accuracy<br>
• Transaction history filtering and pagination<br>
• Boundary and negative amount handling

## What I did

Step 1 — Built decision tables covering every transfer rule (min/max amount, insufficient funds, same-account transfer).<br>
Step 2 — Verified UI balances against the database using SQL (SELECT, JOIN, WHERE) instead of trusting the screen.<br>
Step 3 — Executed 25+ test cases and a dedicated checklist.<br>
Step 4 — Documented every defect with full reproduction evidence.

## Result

• 25+ test cases covering money-flow paths<br>
• 12+ defects logged, including 1 critical balance-mismatch bug caught via SQL cross-check<br>
• All boundary-amount scenarios (0.01 EUR, max transfer, negative input) verified

## Tech / tools

SQL, Decision Tables, Boundary Value Analysis, Jira

---

## Test Cases

Legend: PASS / FAIL / BLOCKED

| ID | Title | Steps | Expected Result | Actual Result | Status |
|---|---|---|---|---|---|
| TC-BN-01 | Transfer minimal valid amount | Transfer 0.01 EUR between own accounts | Both balances update correctly | As expected | PASS |
| TC-BN-02 | Transfer above available balance | Attempt transfer greater than current balance | Blocked with clear error message | As expected | PASS |
| TC-BN-03 | Transfer negative amount | Enter "-50" as amount | Input rejected, validation error shown | As expected | PASS |
| TC-BN-04 | Transfer to non-existent account | Enter invalid IBAN | "Account not found" error shown | As expected | PASS |
| TC-BN-05 | Balance matches DB after transfer | Transfer 20 EUR, query DB balance via SQL | UI balance equals DB balance | Mismatch of 0.02 EUR found | FAIL |
| TC-BN-06 | Transaction history date filter | Filter history by start and end date | Only in-range operations listed | End date ignored, all shown | FAIL |
| TC-BN-07 | Transaction history pagination | Scroll to page 2 of history | Correct next set of transactions loads | As expected | PASS |
| TC-BN-08 | Duplicate transfer submission | Double-click "Confirm" on transfer | Only one transfer is executed | As expected | PASS |
| TC-BN-09 | Currency formatting | Transfer 1234.5 EUR | Displayed as "1,234.50 EUR" | As expected | PASS |
| TC-BN-10 | Session timeout during transfer | Leave transfer form idle 15+ min, submit | Redirected to login, transfer not executed | As expected | PASS |

_Full set (25+ cases) — extend this table with new cases as you test additional flows._

## Checklist

✅ Login and account overview load correctly<br>
✅ Balance shown on dashboard matches database record<br>
✅ Transfer form validates all required fields<br>
⬜ Transfer amount respects both min and max boundaries — see BUG-BN-71<br>
✅ Negative / non-numeric amounts are rejected<br>
✅ Successful transfer updates both sender and receiver balances<br>
✅ Insufficient-funds transfer is blocked with a clear message<br>
⬜ Transaction history date filter respects the end date — see BUG-BN-71<br>
✅ Transaction history pagination works both directions<br>
✅ Duplicate submissions (double-click) don't create duplicate transfers<br>
✅ Session expiry logs the user out safely mid-flow<br>
✅ All monetary values are formatted consistently

## Bug Reports

### BUG-BN-71 — Transaction history filter ignores end date

• Severity: Major<br>
• Priority: Medium<br>
• Environment: Chrome 126, staging<br>
• Steps to reproduce: open Transaction History, set a date range with both start and end date, apply the filter<br>
• Expected result: only transactions within the selected range are shown<br>
• Actual result: all transactions after the start date are shown, end date has no effect<br>
• Status: Open — reported to dev team

### BUG-BN-88 — Balance mismatch after concurrent transfers

• Severity: Critical<br>
• Priority: High<br>
• Environment: Staging, verified via direct SQL query<br>
• Steps to reproduce: initiate two transfers from the same account within 1 second of each other, wait for both to complete, compare UI balance to the database value<br>
• Expected result: UI balance matches the database exactly<br>
• Actual result: UI balance is off by 0.02 EUR — likely a rounding/race condition<br>
• Status: Escalated — flagged as high priority due to financial impact

_Add new bug reports below using the same format._
