# Deployment Validation Report

Date: 2026-10-20 · Tool: Salesforce CLI 2.154.4 · Auth: JWT bearer flow (External Client App "FYZER CLI Access", scopes api + refresh_token, admin pre-authorized only) · Org API version: 68.0

## Retrieve
Command: `sf project retrieve start -x manifest/package.xml -o fyzer`
Result: **66 files retrieved** — CustomObject 6, CustomField 32, Flow 9, PermissionSet 7, PermissionSetGroup 5, GenAiPromptTemplate 1, NamedCredential 1, ValidationRule 1, ListView 4.

## Check-only deployment validation (nothing deployed)
Command: `sf project deploy start --dry-run -o fyzer` (from the retrieved source)
Result: **Status: Succeeded · checkOnly: true · Components deployed 65/65 · Component errors: 0 · Component failures: 0 · Tests: 0 run, 0 errors** (no Apex in scope).

## New-flow validation (QA fixes)
- FYZER_Get_Account_Summary_Data — validated ✅ (after fixing picklist TEXT() formula defect).
- FYZER_Update_Project_Totals_On_Delete — **rejected by the platform** ❌: "The Delete record trigger type isn't compatible with the RecordBeforeSave flow trigger type" and the same for RecordAfterSave; reproduced at project API 62.0 and 68.0. Record-triggered flows on delete are unavailable in this org.
- Full-tree dry-run including the retrieved EntitlementProcess fails on that single component: "Cannot update SLA process that is in use" — expected platform behavior; in-use SLA processes must be versioned in Setup, not updated by deploy (KI-2). All FYZER components validate clean.

## Local project reconciliation
The pre-QA local SFDX project contained 37 files (custom objects and fields only). The retrieve added the 29 remaining files (flows, permission sets/groups, prompt template, named credential, Account object + validation rule, list views). Repo source was synced from the retrieve. One substantive drift found and resolved in the org's favor: Bill_Rate__c precision 18 (org) vs 16 (stale local file). All other diffs are Salesforce XML normalization.

## File-count interpretation

The implementation report records **66 retrieved metadata files** and a **68-file synchronized project view**. These numbers use different inventories/counting conventions; the two extra files cannot be conclusively identified from the report alone. The check-only result of 65/65 is a **metadata component count**, not a filesystem count. Do not present these three counts as interchangeable.
