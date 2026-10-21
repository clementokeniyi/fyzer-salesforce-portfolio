# Known Issues & Limitations

## KI-1 — Milestones do not complete when a case closes (platform behavior)
- **Symptom:** The case page milestone panel keeps counting after closure.
- **Data evidence:** CaseMilestone rows for closed case 00001030: IsCompleted = false, CompletionDate = null, IsViolated = false, ElapsedTimeInMins = null (both First Response and Resolution).
- **Reproduction:** Fresh API-created case 00001034 on BluePeak Industries: entitlement auto-attached, both milestones started, case closed via API — milestones still did not complete or violate.
- **Configuration verified correct** (from retrieved metadata): EntitlementProcess "FYZER Standard Support" is active; exit criteria `Case.IsClosed equals true`; milestones First Response 240 min, Resolution 2,880 min; entry field Case.CreatedDate. Entitlement "FYZER Standard Support - BluePeak" is Active and attached to the cases.
- **Conclusion:** The Developer Edition entitlement engine is not processing milestone completion on closure. This is not a configuration defect and has no safe org-side fix. SLA *targets* and *attachment* work; completion bookkeeping does not. Portfolio reviewers should treat milestone completion as a documented platform limitation of this org.

## KI-2 — In-use SLA processes cannot be updated via metadata deployment
Check-only deploy of the retrieved EntitlementProcess fails with "Cannot update SLA process that is in use". In-use entitlement processes must be changed by creating a new version in Setup. Noted for governance; no action needed.

## TD-1 — PSA totals do not recalculate in real time when a timesheet entry is deleted
Record-triggered flows on delete are unsupported in this org (validation evidence in deployment-validation.md). Total Hours is a native roll-up and stays correct. Total Billed / Total Cost are flow-maintained and can drift after a deletion until reconciled. **Remediation designed:** nightly scheduled reconciliation flow (FYZER_PSA_Totals_Reconciliation) — per-project recompute from remaining entries. Build pending owner approval.

## Other documented items (from the build)
- **TD-2:** "FYZER - Finance Access" permission set belongs to no permission set group (no finance users exist yet); finance tasks are interim-owned by the admin. Intentional, documented.
- **TD-4:** Duplicate unlinked "NoSLA Test Corp" test account retained (deletion needs owner approval).
- **TD-8:** The FYZER Demo Agent (default Employee Agent template) ships with no CRM record actions and answers that it cannot look up records. Remediation flow FYZER_Get_Account_Summary_Data is authored and validated; wiring pending deployment approval.
- **Knowledge:** not yet enabled — the enable toggle does not respond to automation; one manual click by the owner completes it (Setup > Knowledge Settings).
- **Executive dashboard:** "Won Revenue" is a table ($85,000) because Metric charts require a summary-level source report; documented substitution.
- **Case Entitlement FLS:** was hidden for all profiles in this OrgFarm-provisioned org (root cause of early build friction); fixed for System Administrator. When support users are hired, grant Entitlement FLS via the FYZER - Support Access permission set.
- **My Domain** is not FYZER-branded (orgfarm-generated). Cosmetic; changing it post-setup is riskier than the benefit.
