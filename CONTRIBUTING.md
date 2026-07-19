# How to add a new project or new tests

## Adding a brand-new project

Step 1 — Open `00-TEMPLATE-new-project/README.md` and copy its content.<br>
Step 2 — Create a new file named `0N-short-project-name/README.md` (next available number) and paste the copied template into it.<br>
Step 3 — Fill in the Scope, What I did, Result, Test Cases, Checklist and Bug Reports sections.<br>
Step 4 — Add a row for it in the table in the root `README.md`.

## Adding new test cases to an existing project

Open that project's `README.md` and add a new row to the Test Cases table:

`| TC-XX-12 | Short title | Steps to follow | What should happen | What actually happened | PASS/FAIL |`

Keep IDs sequential and prefixed with the project's short code (MP, BN, AU, EC, AP).

## Adding a new bug report

Open that project's `README.md` and copy the bug block format (Severity / Priority / Environment / Steps / Expected / Actual / Status) from the Bug Reports section. Keep bug IDs sequential per project.

## Status and severity conventions used everywhere in this repo

• Status: PASS / FAIL / BLOCKED<br>
• Severity: Critical / Major / Minor / Trivial<br>
• Checklist: ✅ done, ⬜ not yet / known issue

## Note on the /postman folder

The `/postman` folder at the root holds raw Postman collection exports from earlier coursework. It's kept as-is for reference — new structured write-ups (README + test cases + checklist + bug reports) go into the numbered `0N-project-name` folders instead.
