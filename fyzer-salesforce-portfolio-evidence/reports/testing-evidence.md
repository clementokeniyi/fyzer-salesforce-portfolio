# Testing Evidence

## End-to-end tests executed during the build (2026-10-19/20)
- **Lead conversion:** Ava Novak converted → Vertex Analytics account + contact + $57,000 opportunity created correctly.
- **Lead assignment:** Web-to-Lead rules route to the FYZER - Sales Leads queue; catch-all assigns to the sales rep.
- **Won-opportunity handler (both branches):** with SLA date → account becomes Active Customer; without SLA date → Onboarding (live-verified in QA-8 below).
- **Cross-functional handoff:** transition into Active Customer creates CS welcome task, Support health-check case to the support queue, [Finance] invoice task, and a Chatter post. **Idempotency proven:** re-editing the won opportunity / Active Customer account created zero duplicates.
- **Entitlement tests (a–d):** (a) entitlement auto-attaches on case create with a live milestone countdown; (b) negative path: case on an account without entitlement stays clean, no errors; (c) closure Chatter post appears on case close; (d) fault path: a forced flow fault produced exactly ONE FYZER_Automation_Log__c record with the expected INVALID_CROSS_REFERENCE_KEY error.
- **API/integration:** Workbench REST — query 200, POST 201, GET 200, invalid POST 400 INVALID_FIELD. Flow HTTP Callout to the Named Credential endpoint returned "HTTP 200 - delectus aut autem" and logged to FYZER_Automation_Log__c (3 log records total: fault, HTTP callout, REST demo).
- **Quote approval:** Quote 00000003 ($98,500) entered the FYZER Quote Approval process, was approved with comment, and its Approval History trail is populated.
- **PSA math (exact):** BluePeak project — 44 hours, Billed $6,800, Cost $4,320, Profit $2,480, Margin 36.47% — recomputed by flow from timesheet entries and verified to the cent.
- **Duplicate management (live):** entering a second Liam Osei lead fired the "Similar Records Exist" alert during entry; Duplicate Record Set DRS-0000000001 holds both leads.
- **Dashboards:** all five FYZER dashboards verified rendering with correct totals (incl. Executive "Won Revenue" table = $85,000).

## QA-phase tests (2026-10-20, via CLI/API)
- **QA-8 no-SLA branch:** PASSED (see final-qa-results.md). Account "QA NoSLA Branch Test Co" + Closed Won opportunity → Lifecycle "Onboarding", 1 onboarding task, 0 cases, no Finance task.
- **QA-6 milestone reproduction:** test case 00001034 created on BluePeak and closed via API; milestones did not complete (platform behavior, KI-1).
- **Deployment validation:** dry-run Succeeded, 65/65 components, 0 errors.
- **Flow activation audit (Tooling API):** 8/9 FYZER flows active; FYZER_API_Demo_Callout inactive by design.
