# FYZER Salesforce — Governance, Technical Debt & Change Impact

Version 1.0 · 2026-10-20

## 1. Governance model
- **Naming:** all portfolio configuration carries the `FYZER` prefix (labels) / `FYZER_` (API names); simulations carry "(SIMULATION)" in record names and object descriptions.
- **Change discipline:** inspect before changing; preserve existing configuration unless a change is necessary; every change recorded with its purpose in the task log; nothing destructive without owner approval; no item marked complete without Salesforce evidence.
- **Security:** least privilege by permission set groups; credentials only via Named/External Credentials and Secure Vault; fictional data only — the org must never receive real personal data.
- **Automation standards:** transition-only entry conditions; fault paths to the Automation Log; descriptions on every flow stating trigger, guard, and purpose.

## 2. Technical debt register
| # | Debt / limitation | Severity | Position |
|---|---|---|---|
| TD-1 | PSA Total Billed/Cost maintained by a totals flow (create/update only — a timesheet *deletion* would leave stale totals) | Medium | Accepted for the simulation. Production fix: add a delete-context trigger or scheduled reconciliation job; document on the object |
| TD-2 | FYZER - Finance Access permission set belongs to no permission set group; Finance tasks route to the org owner as interim assignee | Low | Revisit when a finance user is hired: create the group assignment and re-point the handoff flow's Get User step |
| TD-3 | FYZER - Support Queue has no members (no support users exist); cases queue correctly but nobody is notified as a member | Low | Add members when support users are created; also grant Case Entitlement FLS to FYZER - Support Access at the same time |
| TD-4 | Duplicate test data in org: a second unlinked "NoSLA Test Corp" account and a deliberate duplicate Liam Osei lead | Low | Left in place under the no-delete rule; merge/delete only with owner approval (the duplicate lead is retained deliberately as duplicate-management evidence) |
| TD-5 | Milestone countdown continues to display after case close (observed platform behavior; exit criteria are configured) | Info | Documented; no workaround attempted. A reporting note warns readers not to treat the panel as proof a milestone is still running |
| TD-6 | Salesforce Knowledge not yet enabled — the enable button does not respond to browser automation | Low | Owner action: Setup > Knowledge Settings > acknowledge > Enable Salesforce Lightning Knowledge |
| TD-7 | "FYZER - API Demo Callout" flow is saved inactive (debug-tested only) | Info | Deliberate: a demo callout should not sit active; activate only if a scheduled demo is wanted |
| TD-8 | FYZER Demo Agent is a Draft and cannot answer CRM record questions (default template has no record actions) | Medium | Backlog: add prompt-template/flow actions + Data Library grounding, human review before activation |
| TD-9 | My Domain is not FYZER-branded (OrgFarm-provisioned name) | Info | Cosmetic only; renaming a My Domain risks link breakage for zero functional gain — leave as-is |

## 3. Legacy automation migration analysis
Inventory found **no legacy Workflow Rules or Process Builder processes** built for this portfolio; all automation is Flow (the modern, supported tool), plus platform rules (assignment, escalation, approval, duplicate). Two "Standard" rule sets (lead/case assignment and case escalation) were superseded by FYZER rules and auto-deactivated by Salesforce when the FYZER rules activated — they remain in place, inactive, as delivered by the org template; no action needed. Recommendation: if the org ever gains Workflow/Process Builder assets, migrate them to Flow before Salesforce's retirement tooling forces the issue; the FYZER flow patterns (transition guards, fault logging) are the target standard.

## 4. Change impact analysis (this build)
- **Profiles:** Customer Account record type made available to System Administrator + Minimum Access; Case Entitlement FLS set Visible for System Administrator only. No other profile changes.
- **Permission sets:** FLS added to FYZER - Sales Access (Read+Edit) and FYZER - Customer Success Access (Read) for the two account SLA/lifecycle fields; one new permission set (FYZER Demo API Access) for credential principal access. No group membership changes.
- **Rules replaced:** Standard lead/case assignment rules and Standard case escalation rule are inactive, superseded by active FYZER equivalents with identical-or-better routing.
- **Data:** all records created are fictional portfolio data; pre-existing FYZER records (Northstar, BluePeak, user accounts) were enriched (lifecycle stages, contacts, opportunities) but never deleted or overwritten destructively. BluePeak's record type changed to Customer Account to satisfy entitlement prerequisites — purposeful and logged.
- **Blast radius:** every automation is entry-guarded; the two organization-wide behaviors a user will notice are (1) outage-subject cases become High priority automatically, and (2) accounts becoming Active Customer spawn three handoff artifacts. Both are the designed behaviors, tested including the edit/idempotency cases.

## 5. Rollback plan
Configuration in this org is additive; rollback is per-component and non-destructive:
1. **Flows:** deactivate the FYZER flow(s) involved (Setup > Flows). No data loss; logs and created tasks/cases remain as history.
2. **Rules:** deactivate FYZER assignment/escalation/duplicate rules; reactivate the Standard rules they superseded.
3. **Approval process:** deactivate FYZER Quote Approval; quotes return to manual status handling.
4. **AI:** deactivate the prompt template version; leave the agent in Draft (it was never activated).
5. **Objects/fields:** custom objects can be taken out of apps/tabs first; deletion is NOT part of rollback without owner approval.
6. **Source-level rollback:** the git history of this repository is the configuration record — revert the relevant commit to identify exactly what to unwind in the org, then apply the matching deactivation above. (Full metadata redeploy rollback via `sf project deploy` requires an authorized org connection — owner action.)
