# 04 · E-commerce Website Testing

**In one sentence:** functional and regression testing of an online store — product pages, cart math, promo codes, and making sure it works properly on a phone, not just a laptop.

## Scope

• Product detail pages and image gallery<br>
• Cart totals, discounts and promo code logic<br>
• Regression pass after every deployment<br>
• Responsive layout across mobile / tablet / desktop

## What I did

Step 1 — Wrote equivalence-partitioning-based test sets for promo codes (valid, expired, wrong case, already used).<br>
Step 2 — Ran exploratory testing sessions using written charters (a short goal plus time box, not just random clicking).<br>
Step 3 — Tracked every defect through its full life cycle in YouTrack.<br>
Step 4 — Executed 35+ test cases across 3 regression cycles.

## Result

• 35+ test cases, 3 regression cycles<br>
• 20+ defects logged, including 1 critical checkout-blocking bug caught before release<br>
• Responsive behavior verified at 375px, 768px and 1440px breakpoints

## Tech / tools

Regression Testing, Smoke Testing, YouTrack, Responsive Testing

---

## Test Cases

Legend: PASS / FAIL / BLOCKED

| ID | Title | Steps | Expected Result | Actual Result | Status |
|---|---|---|---|---|---|
| TC-EC-01 | View product detail page | Open any product from catalog | Images, price, description load correctly | As expected | PASS |
| TC-EC-02 | Apply valid promo code | Enter an active promo code at checkout | Discount applied, total updates | As expected | PASS |
| TC-EC-03 | Apply expired promo code | Enter a promo code past its expiry date | "Code expired" error shown | As expected | PASS |
| TC-EC-04 | Apply promo code in wrong case | Enter code in lowercase when stored as uppercase | Code accepted regardless of case | Code rejected as invalid | FAIL |
| TC-EC-05 | Change quantity to zero in cart | Set item quantity to 0 | Item removed with an undo option | Item stuck at quantity 0, price shows 0.00 | FAIL |
| TC-EC-06 | Cart total after quantity change | Increase quantity from 1 to 3 | Total recalculates immediately | As expected | PASS |
| TC-EC-07 | Out-of-stock product page | Open a sold-out product | "Out of stock" shown, "Add to cart" disabled | As expected | PASS |
| TC-EC-08 | Product page on 375px viewport | Open product page on a small mobile viewport | No horizontal scroll, layout adapts | As expected | PASS |
| TC-EC-09 | Product page on tablet 768px | Open product page at tablet width | Layout adapts, no overlapping elements | As expected | PASS |
| TC-EC-10 | Guest checkout | Complete checkout without creating an account | Order placed successfully as guest | As expected | PASS |
| TC-EC-11 | Wishlist add/remove | Add a product to wishlist, then remove it | Product appears then disappears from wishlist | As expected | PASS |

_Full set (35+ cases across 3 regression cycles) — extend as new features ship._

## Checklist (run before every deploy)

✅ Homepage and catalog load without errors<br>
✅ Product detail page shows correct price, images and stock status<br>
✅ Add to cart works from catalog and product page<br>
⬜ Promo codes are accepted case-insensitively — see BUG-EC-61<br>
⬜ Cart total recalculates correctly when quantity is set to 0 — see BUG-EC-58<br>
✅ Guest checkout completes successfully<br>
✅ Order confirmation page shows correct summary<br>
✅ Layout has no horizontal scroll at 375px width<br>
✅ Layout adapts correctly at 768px (tablet) width<br>
✅ Wishlist add/remove works correctly<br>
✅ No broken images across top 20 products

## Bug Reports

### BUG-EC-58 — Cart total not recalculated after quantity set to 0

• Severity: Critical<br>
• Priority: High<br>
• Environment: Chrome 126, staging<br>
• Steps to reproduce: add any product to the cart, change its quantity field to 0, observe the cart total and line item<br>
• Expected result: item is removed from the cart (optionally with an undo option), total updates accordingly<br>
• Actual result: item remains in the cart at quantity 0 with a price of 0.00, total becomes incorrect<br>
• Status: Fixed and re-verified before release

### BUG-EC-61 — Promo code rejected when entered in lowercase

• Severity: Minor<br>
• Priority: Medium<br>
• Environment: Chrome 126, staging<br>
• Steps to reproduce: go to checkout, enter a valid promo code in lowercase (code is stored as uppercase), apply the code<br>
• Expected result: code is accepted regardless of letter case<br>
• Actual result: "Invalid code" error is shown<br>
• Status: Open — reported to dev team

_Add new bug reports below using the same format._
