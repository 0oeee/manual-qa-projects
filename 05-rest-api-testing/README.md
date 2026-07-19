# 05 · REST API Testing

**In one sentence:** building a Postman collection straight from a Swagger/OpenAPI spec and verifying that the API actually behaves the way its own documentation promises.

> Raw Postman collections from earlier coursework are available in [`/postman`](../postman) at the root of this repo — this folder documents the testing process and findings around that same skill set in a more structured, case-study format.
>
> ## Scope
>
> • CRUD endpoints for orders, users and items<br>
• Auth-protected endpoints (token required)<br>
• Status code and payload contract verification<br>
• Cross-checking API responses against the database
>
> ## What I did
>
> Step 1 — Built a structured Postman collection with 50+ requests (CRUD, auth-protected, negative calls) based on the Swagger spec.<br>
Step 2 — Verified status codes and payload contracts: 200/201 success cases, 400/401/404 error cases.<br>
Step 3 — Cross-checked returned data against the database using SQL queries.<br>
Step 4 — Added basic automated checks with Postman test scripts (status code and schema assertions).
>
> ## Result
>
> • 50+ requests in the Postman collection<br>
• 15+ contract violations / bugs reported with full request/response evidence<br>
• All core CRUD and auth flows verified against the Swagger spec
>
> ## Tech / tools
>
> Postman, Swagger / OpenAPI, REST, SQL, JSON Schema validation
>
> ---
>
> ## Test Cases
>
> Legend: PASS / FAIL / BLOCKED
>
> | ID | Endpoint | Steps | Expected Result | Actual Result | Status |
> |---|---|---|---|---|---|
> | TC-AP-01 | GET /users/id valid id | Send GET with an existing user id | 200 OK, response schema matches Swagger spec | As expected | PASS |
> | TC-AP-02 | GET /users/id invalid id | Send GET with a non-existent id | 404 Not Found with error body | As expected | PASS |
> | TC-AP-03 | POST /orders without auth token | Send POST without Authorization header | 401 Unauthorized | As expected | PASS |
> | TC-AP-04 | POST /orders valid payload | Send POST with valid order body and valid token | 201 Created, order id returned | As expected | PASS |
> | TC-AP-05 | POST /orders missing required field | Omit "quantity" from request body | 400 Bad Request with field-level error | As expected | PASS |
> | TC-AP-06 | PUT /items/id update | Update an existing item's price | 200 OK, updated value reflected on next GET | As expected | PASS |
> | TC-AP-07 | DELETE /items/id first call | Delete an existing item | 200 OK, item removed | As expected | PASS |
> | TC-AP-08 | DELETE /items/id second call | Repeat the same DELETE request | 404 Not Found (already deleted) | Returns 500 Internal Server Error | FAIL |
> | TC-AP-09 | Response time GET /users | Measure response time for list endpoint | Under 300ms on staging | As expected, avg 154ms | PASS |
> | TC-AP-10 | Data consistency order total | Compare GET /orders/id total to SQL SELECT from DB | Values match exactly | As expected | PASS |
> | TC-AP-11 | Pagination GET /orders page 2 | Request page 2 of results | Returns correct next page, no duplicate items | As expected | PASS |
>
> _Full collection (50+ requests) — extend with new endpoints and negative cases as the API grows._
>
> ## Checklist
>
> ✅ All CRUD endpoints return correct status codes for happy-path requests<br>
✅ Auth-protected endpoints reject requests without a valid token (401)<br>
✅ Required-field validation returns 400 with a clear error body<br>
✅ Not-found resources return 404, not 500<br>
⬜ Repeated DELETE on an already-deleted resource returns 404, not 500 — see BUG-AP-90<br>
✅ Response schemas match the Swagger/OpenAPI definitions<br>
✅ Response times stay under the agreed threshold (300ms) on staging<br>
✅ Returned totals/amounts match the underlying database values<br>
✅ Pagination does not skip or duplicate records across pages
>
> ## Bug Reports
>
> ### BUG-AP-90 — Repeated DELETE returns 500 instead of 404
>
> • Severity: Major<br>
• Priority: High<br>
• Environment: Postman, staging environment<br>
• Steps to reproduce: send DELETE /items/id for an existing item (succeeds with 200 OK), then send the exact same DELETE request again<br>
• Expected result: second call returns 404 Not Found (resource no longer exists)<br>
• Actual result: second call returns 500 Internal Server Error<br>
• Status: Open — reported to dev team with request/response payloads attached
>
> _Add new bug reports below using the same format._
> 
