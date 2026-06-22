# Sync and Ownership Rules

## Purpose
This document defines ownership, synchronization, and lifecycle rules across the Sales Cockpit ecosystem.

The goal is to ensure:
- every important field has a clear primary owner
- systems do not overwrite each other unpredictably
- Salesforce, the BVA app, Power BI, and repository artifacts operate coherently
- future implementation sources can be added without creating semantic conflicts

This document works with:
- `docs/sales-cockpit-operating-model.md`
- `docs/cockpit-data-model.md`
- `docs/system-mapping-salesforce-powerbi.md`

## Core principle
Each canonical field or artifact domain should have:
1. a **primary system of record**
2. a **sync direction**
3. a **consumer set**
4. a **conflict rule**

## Rule hierarchy
When conflicts exist, resolve them in this order:
1. canonical business meaning
2. primary system of record
3. approved sync rule
4. local implementation logic

Implementation convenience must not override canonical semantics.

## Systems in scope

### Salesforce
Primary role:
- CRM master data
- account and opportunity lifecycle
- stakeholder relationships
- selected qualification data

### BVA app
Primary role:
- BVA workflow execution
- BVA phase progression
- value model content
- simulation mode
- executive-readout generation

### Sales Cockpit repository
Primary role:
- durable artifacts
- templates
- operating memory
- linked working outputs
- structured documentation

### Power BI
Primary role:
- analytics
- snapshots
- trend reporting
- portfolio rollups
- derived inspection views

### Future implementation sources
Primary role:
- domain-specific extensions
- specialized workflow or calculations
- should map to the canonical model without redefining it

## Ownership model by domain

## 1. Account domain

### Primary system of record
- Salesforce

### Canonical fields typically owned here
- `account_id`
- `account_name`
- `industry`
- `segment`
- `region`
- `account_owner`

### Sync direction
- Salesforce → cockpit consumers
- Salesforce → Power BI
- Salesforce → future integrated systems as needed

### Conflict rule
If account identity fields differ across systems, Salesforce wins.

### Exception
Cockpit-local notes about account strategy may exist outside Salesforce, but should not overwrite account master identity.

## 2. Opportunity domain

### Primary system of record
- Salesforce

### Canonical fields typically owned here
- `opportunity_id`
- `opportunity_name`
- `opportunity_stage`
- `amount`
- `target_close_date`
- `forecast_category`
- `opportunity_owner`

### Sync direction
- Salesforce → cockpit consumers
- Salesforce → Power BI
- optional Salesforce → BVA app context feed

### Conflict rule
Commercial opportunity master data should not be edited outside the CRM master workflow unless an approved integration pattern exists.

## 3. Engagement domain

### Primary system of record
- Cockpit layer / custom engagement implementation

### Canonical fields typically owned here
- `engagement_id`
- `engagement_name`
- `engagement_type`
- `engagement_owner`
- `strategic_objective`
- `next_milestone`
- `next_action`
- `status`
- `source_entry_path`
- `summary`

### Sync direction
- cockpit layer ↔ Salesforce custom engagement implementation, if present
- cockpit layer → Power BI
- cockpit layer → BVA app for contextual linkage where needed

### Conflict rule
If Salesforce contains a mapped engagement object, one implementation must still be declared authoritative for each field.
Do not allow bidirectional free-edit without field-level ownership.

## 4. BVA domain

### Primary system of record
- BVA app

### Canonical fields typically owned here
- `bva_id`
- `bva_status`
- `bva_phase`
- `bva_scenario_type`
- `bva_last_updated`
- `business_problem_summary`
- `baseline_metrics_summary`
- `value_drivers_summary`
- `quantified_outcomes_summary`
- `assumptions_summary`
- `confidence_level`
- `risks_dependencies_summary`
- `executive_summary`

### Sync direction
- BVA app → Salesforce metadata layer
- BVA app → cockpit engagement record summary fields where needed
- BVA app → repository artifacts or exported outputs
- BVA app → Power BI for reporting

### Conflict rule
BVA content fields should not be edited in Salesforce as competing master copies.
Salesforce may hold metadata, references, and reporting fields.

### Exception
A manually curated executive summary may be maintained as a separate artifact, but should be linked distinctly rather than silently replacing app output.

## 5. MEDDPICC domain

### Primary system of record
- must be explicitly chosen by implementation
- recommended default: Salesforce if embedded in opportunity inspection
- alternative: cockpit layer if managed as its own operating object

### Canonical fields
- `meddpicc_status`
- `metrics_status`
- `economic_buyer_status`
- `decision_criteria_status`
- `decision_process_status`
- `paper_process_status`
- `identify_pain_status`
- `champion_status`
- `competition_status`

### Sync direction
If Salesforce-owned:
- Salesforce → cockpit consumers
- Salesforce → Power BI

If cockpit-owned:
- cockpit layer → Salesforce summary fields if required
- cockpit layer → Power BI

### Conflict rule
Do not split component ownership across systems unless there is a very explicit and controlled reason.
MEDDPICC fragmentation is a major risk.

## 6. Executive prep domain

### Primary system of record
- cockpit layer or structured CRM implementation
- artifact content often owned by repository

### Canonical fields
- `exec_prep_status`
- `exec_event_type`
- `exec_event_date`
- `exec_objective`
- `exec_key_message`

### Sync direction
- cockpit layer / CRM object → Power BI
- repository artifact links → Salesforce and cockpit references as needed
- BVA app outputs may feed executive prep content, but should not automatically own all executive prep data

### Conflict rule
Structured status may live in CRM/cockpit records, while the actual briefing artifact may live in the repository.
These should be linked, not duplicated as uncontrolled parallel versions.

## 7. Portfolio domain

### Primary system of record
- Power BI for analytical portfolio views
- cockpit layer may own planning annotations if needed

### Canonical fields
- `portfolio_priority`
- `portfolio_theme`
- `portfolio_risk_flag`
- `leadership_attention_required`
- `portfolio_notes`

### Sync direction
- Salesforce, cockpit, and BVA app feed Power BI
- Power BI produces derived portfolio views
- portfolio annotations may flow back only if there is a defined operational need

### Conflict rule
Power BI should not be used as a writeback master unless a deliberate writeback architecture exists.

## 8. Artifact domain

### Primary system of record
- repository and/or app storage depending on artifact type

### Recommended ownership pattern
- source documents live where they are authored
- Salesforce stores links and lightweight metadata
- Power BI consumes metadata only, not full document bodies

### Typical artifact ownership examples
- BVA exports → BVA app or repository
- executive briefs → repository
- MEDDPICC scorecards → repository or CRM-linked object
- account plans → repository
- portfolio review decks → repository

### Conflict rule
Avoid storing full artifact content in multiple systems as competing versions.
Prefer a single artifact host plus references.

## Sync direction patterns

## Pattern 1: Master to consumer
Use when one system clearly owns the field.

Examples:
- Salesforce account data → Power BI
- BVA phase → Salesforce metadata
- BVA status → cockpit summary

## Pattern 2: Master to reporting layer
Use for reporting-only consumers.

Examples:
- opportunity stage → Power BI
- engagement status → Power BI
- MEDDPICC status → Power BI

## Pattern 3: Reference sync
Use when only links or metadata should move.

Examples:
- repository brief link → Salesforce URL field
- BVA artifact link → engagement record
- executive brief path → portfolio review reference

## Pattern 4: Controlled summary sync
Use when detailed content stays in one system, but summarized fields are synchronized elsewhere.

Examples:
- BVA app detailed outputs remain in app/repository
- summarized confidence/risk/status sync to CRM and reporting tools

## Write rules

### Rule 1
A consumer system may display a field without becoming its owner.

### Rule 2
A consumer system must not silently overwrite a field owned elsewhere.

### Rule 3
If writeback is allowed, it must be field-specific and explicitly documented.

### Rule 4
Free-form text fields should have stricter ownership rules than simple status fields.

### Rule 5
Derived reporting fields should not sync back into operational systems unless explicitly approved.

## Conflict resolution rules

## Field-level conflict
When the same field value differs between systems:
1. check canonical owner
2. check latest valid sync event
3. prefer system-of-record value
4. log exception if mismatch persists

## Artifact conflict
When two artifacts appear to represent the same thing:
1. determine primary artifact host
2. retain one canonical artifact
3. convert the other to a reference or archive version
4. avoid dual-active versions unless deliberately versioned

## Status conflict
If summarized status differs from detailed source state:
- detailed source system wins
- summary systems should be refreshed
- manual overrides must be explicit and time-bound

## Future tool onboarding rule
Before onboarding a new tool, define:
- which canonical objects it touches
- which fields it owns
- which fields it only consumes
- whether it creates artifacts
- how conflicts are resolved
- whether it introduces new local-only fields

No new tool should be onboarded without this mapping.

## Recommended ownership matrix

| Domain | Primary Owner | Secondary Consumers | Writeback Allowed |
|---|---|---|---|
| Account | Salesforce | Cockpit, Power BI, future tools | Limited, controlled |
| Opportunity | Salesforce | Cockpit, BVA app, Power BI | Limited, controlled |
| Engagement | Cockpit layer | Salesforce custom object, Power BI | Yes, field-specific |
| BVA workflow | BVA app | Salesforce metadata, repository, Power BI | Limited, controlled |
| MEDDPICC | Salesforce or cockpit layer | Power BI, other consumers | Only if explicitly chosen |
| Executive prep status | Cockpit / CRM structure | Power BI, repository references | Yes, structured only |
| Portfolio reporting | Power BI | leadership consumers | No by default |
| Artifact bodies | repository / source app | Salesforce links, Power BI metadata | No, use references |

## Recommended implementation rule
When uncertain, prefer:
- one master
- many consumers
- reference sync over duplication
- summary sync over full text duplication
- explicit writeback over implicit writeback

## Summary
The Sales Cockpit ecosystem should operate with clear ownership and controlled synchronization.

The most important principles are:
- one primary owner per field domain
- no uncontrolled parallel masters
- repository artifacts linked rather than duplicated
- reporting systems consume more than they write
- future tools map into the model rather than redefining it
