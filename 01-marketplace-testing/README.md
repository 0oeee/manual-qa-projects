# 01 · Marketplace Testing

**In one sentence:** manual testing of a training marketplace web app — search, filters, cart and checkout — to make sure a customer can find and buy a product without surprises.

## Scope

• Catalog browsing, search and filtering (price, category, sorting)<br>
• Cart: add / remove / update quantity, stock validation<br>
• Checkout: address form, order confirmation<br>
• Cross-browser check on Chrome and Firefox

## What I did

Step 1 — Read through the app like a real buyer first, noting anything confusing or inconsistent.<br>
Step 2 — Wrote 40+ test cases covering the full path from search to order confirmation.<br>
Step 3 — Built a smoke checklist for quick pre-release checks.<br>
Step 4 — Ran a full regression pass after each new build.<br>
Step 5 — Logged every defect found with steps, severity and priority.

## Result

• 40+ test cases written and executed<br>
• 25+ defects logged, including 1 critical checkout blocker<br>
• Purchase funnel (search to cart to checkout to confirmation) fully covered<br>
• Price filter bug (BUG-MP-104) fixed and re-verified before sign-off

## Tech / tools

Test Cases, Checklists, Jira, Chrome DevTools, Cross-browser testing

---

## Test Cases

Legend: PASS / FAIL / BLOCKED

| ID | Title | Steps | Expected Result | Actual Result | Status |
|---|---|---|---|---|---|
| TC-MP-01 | Search returns relevant results | Open catalog, search "wireless mouse" | Only relevant products shown | As expected | PASS |
| TC-MP-02 | Search with no matches | Search for "asdkjaskjd123" | "No results found" message shown | As expected | PASS |
| TC-MP-03 | Apply price filter 0-10 EUR | Set filter range to 0-10 EUR | Only items under 10 EUR listed | Items up to 12 EUR shown | FAIL |
| TC-MP-04 | Sort by price ascending | Select "Price: Low to High" | List sorted ascending by price | As expected | PASS |
| TC-MP-05 | Add in-stock item to cart | Click "Add to cart" on available item | Item appears in cart, count badge updates | As expected | PASS |
| TC-MP-06 | Add out-of-stock item to cart | Click "Add to cart" on sold-out item | Warning shown, item not added | As expected | PASS |
| TC-MP-07 | Update quantity in cart | Change quantity from 1 to 3 | Line total recalculates instantly | As expected | PASS |
| TC-MP-08 | Remove item from cart | Click remove icon on a cart line | Item removed, totals update | As expected | PASS |
| TC-MP-09 | Checkout with empty address field | Leave "City" empty, submit | Inline validation error shown | As expected | PASS |
| TC-MP-10 | Checkout with valid data | Fill all required fields, submit | Order confirmation page shown | As expected | PASS |
| TC-MP-11 | Order confirmation email | Complete an order | Confirmation email received within 2 min | As expected | PASS |
| TC-MP-12 | Catalog page on Firefox | Repeat TC-MP-05 to TC-MP-10 on Firefox | Same behavior as Chrome | Cart badge doesn't update on Firefox | FAIL |

_Full set (40+ cases) covers boundary values, empty states, pagination and mobile breakpoints. Add new executed cases below as this project grows._

## Checklist (run before every release candidate)

✅ Homepage loads without console errors<br>
✅ Search bar returns results for a known product<br>
✅ Category filters apply correctly<br>
⬜ Price filter respects both bounds — known issue, see BUG-MP-104<br>
✅ Sorting (price, popularity, newest) works in both directions<br>
✅ "Add to cart" works from catalog and from product page<br>
✅ Cart badge count updates on add/remove<br>
✅ Out-of-stock items cannot be added to cart<br>
✅ Cart persists after page refresh<br>
✅ Checkout form validates required fields<br>
✅ Order confirmation page shows correct order summary<br>
✅ Confirmation email is sent<br>
⬜ Cart badge updates correctly on Firefox — known issue, see BUG-MP-105<br>
✅ No horizontal scroll on mobile viewport (375px)<br>
✅ Page load under 3s on throttled 4G

## Bug Reports

### BUG-MP-104 — Price filter includes items above the upper bound

• Severity: Major<br>
• Priority: High<br>
• Environment: Chrome 126, desktop, staging<br>
• Steps to reproduce: open catalog, set price filter to 0-10 EUR, observe results<br>
• Expected result: only items priced 10 EUR or less are shown<br>
• Actual result: items priced up to 12 EUR appear in the results<br>
• Status: Fixed and re-verified in build 1.4.2

### BUG-MP-105 — Cart badge does not update on Firefox

• Severity: Minor<br>
• Priority: Medium<br>
• Environment: Firefox 128, desktop, staging<br>
• Steps to reproduce: open catalog on Firefox, add any item to cart, observe cart icon badge<br>
• Expected result: badge count increments immediately<br>
• Actual result: badge stays at 0 until page reload<br>
• Status: Open — reported to dev team

_Add new bug reports below using the same format._
