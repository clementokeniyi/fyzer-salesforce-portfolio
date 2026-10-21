# Completion Checklist — FYZER Salesforce Portfolio (30 items)

Status key: ✅ verified with live org evidence · ⚠️ complete with a documented limitation · 📋 owner decision pending

## Foundation & security
1. ✅ Org setup audited (Developer Edition, timezone/locale verified)
2. ✅ Role hierarchy — 14 roles matching the FYZER org chart
3. ✅ Permission sets — 7 FYZER sets (incl. Demo API Access), least-privilege composed
4. ✅ Permission set groups — 5 job-role groups
5. ✅ OWD Private (Lead/Account/Opportunity/Case/Contract) + targeted sharing rule
6. ✅ Least-privilege test users (Minimum Access profile) verified
7. ✅ Customer Lifecycle Stage field (7 stages, Prospect default)
8. ✅ Validation rule FYZER Active Customer Requires SLA (active, both branches live-tested)
9. ✅ Record types (Prospect/Customer) + FYZER Account Management Lightning page

## Sales
10. ✅ Lead assignment (queue + catch-all) and Web-to-Lead
11. ✅ Lead conversion verified (Ava Novak → Vertex Analytics)
12. ✅ Accounts/contacts demo dataset (9 accounts)
13. ✅ Opportunity pipeline + Path ($225,000 open)
14. ✅ Products + price books (Standard + Enterprise at 10% off)
15. ✅ FYZER Sales Dashboard rendering live data

## Service
16. ✅ Service Console app + FYZER Case Record Page
17. ✅ Case assignment to Support Queue + 4-hour escalation rule
18. ⚠️ Entitlements & milestones configured and attaching correctly; milestone *completion on closure* does not process in this org (KI-1, platform behavior, reproduced and documented)
19. ✅ FYZER Service Dashboard (⚠️ Knowledge was manually enabled per the owner; verify it in Setup)

## Automation
20. ✅ Record-triggered flows (priority guard, auto-entitlement, closure note) active with fault logging
21. ✅ Customer Onboarding screen flow + reusable subflow
22. ✅ Cross-functional automation (Won handler + Active-Customer handoff; idempotency proven)

## Simulations (labeled, never claimed as licensed products)
23. ✅ Quote-to-Cash simulation — quotes, $50k+ approval process, activated Order + Contract
24. ⚠️ PSA simulation — projects/resources/timesheets/milestones with exact verified profitability; delete-recalc is not real-time (TD-1: delete-triggered flows unsupported in this org; nightly reconciliation designed, build pending approval)

## Integrations & data quality
25. ✅ Named Credential + Flow HTTP Callout + Workbench REST evidence (200/201/400 contract tested)
26. ✅ Duplicate matching + duplicate rules — live test fired the alert; Duplicate Record Set created
27. ✅ Data dictionary published (documentation/)

## AI & analytics
28. ✅ Executive + Customer Success + Admin dashboards (5 total, all rendering)
29. ⚠️ Agentforce — Einstein GenAI on; FYZER Account Summary prompt template active and preview-tested; demo agent has no CRM actions in its template (TD-8) — remediation flow authored + validated, deployment pending owner approval

## DevOps & documentation
30. ✅ SFDX project synced from org retrieve (68 files), check-only deployment validation passed (65/65, 0 errors), git history with branch+merge, full documentation bundle, this evidence package

## Owner decisions outstanding
- Verify Lightning Knowledge enabled (owner reports manual activation on 2026-10-20)
- Approve deployment of FYZER_Get_Account_Summary_Data + Agentforce wiring (item 29)
- Approve building the PSA nightly reconciliation flow (item 24)
- Decide on deleting the duplicate unlinked "NoSLA Test Corp" test account
- Approve the "web" OAuth scope addition for screenshot capture (if not already actioned)
- Approve GitHub publication (nothing has been pushed)
