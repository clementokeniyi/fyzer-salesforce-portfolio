# Proposed flows (not deployed)

Authored during the 2026-10-20 QA phase. None of these are in the org yet.

| Flow | Purpose | Validation status |
|------|---------|-------------------|
| FYZER_Get_Account_Summary_Data | Agentforce action: returns industry + lifecycle stage + open-case count for an account name (fixes TD-8) | ✅ Passed check-only deployment validation. Deploy + wire into FYZER Demo Agent pending owner approval. |
| FYZER_Update_Project_Totals_On_Delete | Real-time PSA totals recalculation when a timesheet entry is deleted (TD-1) | ❌ Rejected by the platform: record-triggered flows on delete are unsupported in this org (both trigger phases, API 62 & 68). Kept as the design record. |
| FYZER_PSA_Totals_Reconciliation | Nightly scheduled per-project totals recompute (the supported TD-1 remediation) | ⚠️ Design complete; hand-authored XML hit an XSD element-order rejection — build in Flow Builder UI pending owner approval. |
