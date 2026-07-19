# 03 · Authentication System Testing

**In one sentence:** testing registration, login, password reset and session handling — with an emphasis on what happens when things go wrong, not just when they go right.

## Scope

• Registration (valid/invalid data, duplicate accounts)<br>
• Login (correct/incorrect credentials, lockout behavior)<br>
• Password reset flow end-to-end<br>
• Session expiry and re-authentication

## What I did

Step 1 — Designed negative test sets: unusual input (extra spaces, unicode, injection-like strings, empty fields).<br>
Step 2 — Verified error messages, account lockout behavior and validation UX for each case.<br>
Step 3 — Checked cookie/session behavior directly in Chrome DevTools (Application tab).<br>
Step 4 — Executed 30+ test cases and logged every inconsistency.

## Result

• 30+ authentication scenarios covered, including negative/edge cases<br>
• 10+ issues documented, including a login-blocking input-trimming bug<br>
• Session expiry and lockout behavior verified against expected security rules

## Tech / tools

Negative Testing, Chrome DevTools, Checklists, YouTrack

---

## Test Cases

Legend: PASS / FAIL / BLOCKED

| ID | Title | Steps | Expected Result | Actual Result | Status |
|---|---|---|---|---|---|
| TC-AU-01 | Register with valid data | Fill form with valid unique email/password | Account created, redirected to dashboard | As expected | PASS |
| TC-AU-02 | Register with existing email | Register using an already-registered email | "Email already in use" error shown | As expected | PASS |
| TC-AU-03 | Register with weak password | Enter password "123" | Validation error: password too weak | As expected | PASS |
| TC-AU-04 | Login with correct credentials | Enter valid email/password | Redirected to dashboard | As expected | PASS |
| TC-AU-05 | Login with incorrect password | Enter valid email, wrong password | Generic "invalid credentials" error | As expected | PASS |
| TC-AU-06 | Login with trailing space in email | Enter " user@mail.com " with spaces | Input trimmed automatically, login succeeds | Login rejected as invalid credentials | FAIL |
| TC-AU-07 | Account lockout after failed attempts | Enter wrong password 5 times in a row | Account temporarily locked, message shown | As expected | PASS |
| TC-AU-08 | Password reset valid email | Request reset for a registered email | Reset link email sent within 2 minutes | As expected | PASS |
| TC-AU-09 | Password reset unregistered email | Request reset for a non-existent email | Generic confirmation shown, no account enumeration | As expected | PASS |
| TC-AU-10 | Password reset link expiry | Use a reset link older than 24h | Link rejected, prompted to request a new one | As expected | PASS |
| TC-AU-11 | Session expiry | Leave app idle past session timeout, reload | Redirected to login screen | As expected | PASS |
| TC-AU-12 | SQL-injection-like input in login | Enter apostrophe-OR-1=1 style string as email | Input rejected as invalid, no unexpected behavior | As expected | PASS |

_Full set (30+ cases) — extend with additional negative and cross-browser cases as needed._

## Checklist

✅ Registration rejects duplicate emails<br>
✅ Password strength rules enforced client- and server-side<br>
✅ Login error messages don't reveal whether email or password was wrong<br>
⬜ Email input is trimmed before validation — see BUG-AU-33<br>
✅ Account locks after repeated failed login attempts<br>
✅ Password reset does not confirm/deny account existence<br>
✅ Reset links expire after a reasonable time window<br>
✅ Session expires after inactivity and forces re-login<br>
✅ No sensitive data visible in DevTools network tab in plaintext<br>
✅ Logout clears session cookie completely<br>
✅ Basic injection-style input is safely rejected, not executed

## Bug Reports

### BUG-AU-33 — Email with trailing space rejected as invalid credentials

• Severity: Minor<br>
• Priority: Medium<br>
• Environment: Chrome 126, staging<br>
• Steps to reproduce: go to login page, enter a valid email with a trailing space, enter the correct password and submit<br>
• Expected result: input is trimmed automatically, login succeeds<br>
• Actual result: login fails with "invalid credentials", even though the credentials are correct<br>
• Status: Open — low effort fix, recommended for next sprint

_Add new bug reports below using the same format._
