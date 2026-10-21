# FYZER Salesforce — Admin Standard Operating Procedure (SOP)

Audience: the FYZER Salesforce administrator. Org: fyzer.co Developer Edition.

## Daily / weekly checks
1. Open the **FYZER Admin Dashboard**: check *Automation Log by Flow* for new records — each one is a flow fault or an integration result that needs a look.
2. Check **FYZER Automation Log** list: triage any record whose Error Message isn't an expected demo result ("HTTP 200…", "REST API demo…"). Note the Flow Name and Related Record, open the flow's debug runs, fix, and re-test.
3. Review **Duplicate Record Sets** for new sets created by the duplicate rules; merge confirmed duplicates (Leads) using the Potential Duplicate Records merge tool. Never merge without checking both records' activity.

## User & access management
- New users get the **Minimum Access - Salesforce** profile plus the permission set group for their job role (Sales Representative, Sales Manager, Customer Success Manager, Support Agent, Support Manager). Never assign FYZER permission sets directly — groups are the unit of assignment.
- Finance users: assign FYZER - Finance Access via a group when the role is created (today it sits outside groups and finance tasks route to the org owner — see Governance TD-2).
- Support users: add them to the **FYZER - Support Queue** and confirm the FYZER - Support Access permission set exposes the Case Entitlement field before their first shift.
- Roles mirror the org chart; create the role before the user so reporting lines (dashboards, hierarchy sharing) work from day one.

## Lead operations
- Web leads land in **FYZER - Sales Leads**; the queue owner works them FIFO and reassigns to a rep. Non-web leads fall back to the sales rep via the assignment rule's catch-all.
- If duplicate alerts fire at entry, reps should open the existing lead instead of saving a copy; admins review the Duplicate Record Set weekly.
- Converting: use the standard Convert action; choose the correct Account record type (Prospect vs Customer); always set the primary contact role on the new opportunity.

## Case operations
- Cases route via **FYZER Case Assignment** (Web → Support Queue; catch-all → Support Queue).
- Any subject mentioning an outage/down is auto-raised to High priority by the Priority Guard flow — do not lower it back manually without a note.
- High-priority open cases escalate after 4 hours (reassign + owner email). Early triggers are off in this org, so escalation evaluates on the standard hourly cycle — set expectations accordingly.
- Entitled customers: the entitlement attaches automatically when the case is created blank. If Entitlement Name is empty on a BluePeak case, check (1) the account has an active entitlement covering today, (2) the auto-entitlement flow is active, (3) the Automation Log for a fault — in that order.
- Closing an entitled case posts the SLA audit note automatically; don't delete the post — it's the audit trail.

## Change management
1. Build in Setup or Flow Builder; name everything `FYZER`-prefixed; write a description (trigger, guard, purpose) on every flow/rule.
2. Test with fictional data only: happy path, negative path, and an edit-the-record idempotency check for any automation that creates records.
3. Record the change + test evidence in the task log (`logs/task-log.md`) and update the checklist and data dictionary in the same commit.
4. Destructive changes (deletions, merges of non-test data, permission broadening) require the org owner's explicit approval first.
5. Rollback = deactivate, don't delete (see Governance §5).

## Release process
- Portfolio source lives in this repository (`sfdx/` manifest + `docs/`). Commit messages state the org change they represent.
- Validating against an org requires an authorized Salesforce CLI connection (owner action): `sf org login web`, then `sf project deploy start --dry-run --manifest sfdx/manifest/package.xml`.
- Never push this repository to a public remote without the owner's approval.

## AI usage
- The **FYZER Account Summary** prompt template generates a draft brief on Account Description — a human reviews before anything is sent to a customer.
- The **FYZER Demo Agent** stays in Draft until it has record actions and a human-review process (see Governance TD-8). Do not activate it for users yet.
