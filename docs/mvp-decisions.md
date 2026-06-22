# MVP Decisions

## Purpose
This document records the final MVP decisions required to move from architecture into implementation.

These decisions finalize:
- Engagement MVP fields
- system-of-record choices
- first sync assumptions
- direct UI edit boundaries

This document is the decision companion to:
- `docs/mvp-implementation-blueprint.md`
- `docs/cockpit-data-model.md`
- `docs/system-mapping-salesforce-powerbi.md`
- `docs/sync-and-ownership-rules.md`

## Decision status
All decisions in this document are considered **approved for MVP implementation** unless explicitly replaced by a later decision log entry.

---

## 1. Engagement MVP fields are frozen enough

### Decision
The following fields are **frozen for MVP implementation** and must be supported in the first version of the Cockpit UI and backing model.

### Required Engagement fields
- `engagement_id`
- `engagement_name`
- `engagement_type`
- `engagement_owner`
- `account_id`
- `strategic_objective`
- `next_milestone`
- `next_action`
- `status`
- `bva_status`
- `meddpicc_status`
- `exec_prep_status`

### Required linked display fields
- `account_name`
- `opportunity_id` when applicable
- `opportunity_name` when applicable
- `opportunity_stage` when applicable
- `bva_phase` when applicable
- `bva_last_updated` when applicable
- `exec_event_type` when applicable
- `exec_event_date` when applicable

### Optional but approved for MVP if easy
- `source_entry_path`
- `summary`
- `last_updated_date`
- `bva_summary_artifact`
- `meddpicc_summary_artifact`
- `exec_brief_artifact`

### Deferred from MVP
The following are explicitly deferred from MVP unless needed later:
- detailed MEDDPICC component ownership and scoring
- stakeholder-role orchestration
- artifact lifecycle states beyond simple link/reference support
- advanced portfolio scoring logic
- complex writeback metadata

### Rationale
This field set is sufficient to:
- create and manage an engagement
- view opportunity context
- track BVA progress
- track MEDDPICC summary status
- track executive readiness
- link to working artifacts

---

## 2. Source-of-truth decisions are made

## Decision summary
The following systems are the approved MVP systems of record.

| Domain | MVP System of Record | Notes |
|---|---|---|
| Account | Salesforce | CRM master |
| Opportunity | Salesforce | CRM master |
| Engagement | Cockpit app layer | primary operating object |
| BVA workflow and content | BVA app | cockpit consumes summary metadata |
| MEDDPICC summary status | Cockpit app layer | chosen for MVP simplicity and UI coherence |
| Executive prep summary status | Cockpit app layer | linked to artifacts as needed |
| Portfolio reporting | Power BI | derived consumption layer |
| Artifact bodies | repository or source app | cockpit stores references |

### Specific decision: Engagement lives first in the Cockpit app layer
The first operational home of Engagement is the **Cockpit web app backing store/service**, not Salesforce.

### Why
This keeps the Cockpit as the real operating layer and avoids overcoupling MVP behavior to CRM custom-object design too early.

### Specific decision: `meddpicc_status` is owned by the Cockpit app layer in MVP
For MVP, `meddpicc_status` is a cockpit-owned summary field.

### Why
This avoids blocking the UI on deeper Salesforce qualification design and allows the engagement workspace to be coherent from day one.

### Specific decision: account and opportunity remain read-through CRM references
The Cockpit will not own account or opportunity masters in MVP.
It consumes them from Salesforce.

---

## 3. First sync assumptions are stable

## Approved MVP sync model
The MVP uses a **read-oriented and summary-oriented sync model**, not a deep bidirectional sync model.

### Sync A: Salesforce → Cockpit
#### Fields consumed
- `account_id`
- `account_name`
- `industry` if available
- `segment` if available
- `region` if available
- `account_owner` if needed
- `opportunity_id`
- `opportunity_name`
- `opportunity_stage`
- `amount` if needed later
- `target_close_date` if needed later
- `forecast_category` if needed later
- `opportunity_owner` if needed

#### Direction
- one-way into cockpit views and context resolution

#### MVP assumption
CRM context is readable and trustworthy enough to support the engagement workspace.

### Sync B: BVA app → Cockpit
#### Fields consumed
- `bva_id`
- `bva_status`
- `bva_phase`
- `bva_last_updated`
- `bva_summary_artifact` or equivalent primary link

#### Direction
- one-way summary sync into cockpit

#### MVP assumption
Detailed BVA content remains in the BVA app and/or artifact host.
The cockpit only needs summary visibility.

### Sync C: Cockpit → Power BI
#### Fields published
- engagement fields
- BVA summary fields
- cockpit-owned `meddpicc_status`
- cockpit-owned `exec_prep_status`
- artifact availability indicators
- optional simple portfolio annotations if added

#### Direction
- one-way to reporting layer

#### MVP assumption
Power BI is a reporting consumer, not a writeback system.

### Sync D: Repository / artifact references → Cockpit
#### Fields consumed
- artifact path or URL
- artifact type
- linked entity metadata

#### Direction
- reference-level integration only

#### MVP assumption
The cockpit does not manage full document storage itself in MVP.

### Explicit non-goals for MVP sync
- no unrestricted bidirectional sync
- no Power BI writeback
- no full artifact-content replication
- no parallel master copies of BVA text content

---

## 4. We know what the UI edits directly

## Approved UI editing model for MVP
The Cockpit web app is the **direct editing surface** for engagement workflow fields and selected summary coordination fields.

### Editable in Cockpit UI
- `engagement_name`
- `engagement_type`
- `engagement_owner`
- `strategic_objective`
- `next_milestone`
- `next_action`
- `status`
- `summary`
- `source_entry_path`
- `meddpicc_status`
- `exec_prep_status`
- `exec_event_type`
- `exec_event_date`
- artifact references created by cockpit users

### Read-only in Cockpit UI
- `account_id`
- `account_name`
- account master fields from Salesforce
- `opportunity_id`
- `opportunity_name`
- `opportunity_stage`
- other CRM master fields
- `bva_id`
- `bva_status`
- `bva_phase`
- `bva_last_updated`
- detailed BVA content fields sourced from the BVA app
- Power BI derived metrics

### Link-out behavior
The Cockpit UI should allow users to open:
- Salesforce account/opportunity context
- BVA app or BVA artifact
- repository artifacts
- reporting views if needed

### Why this edit model is approved
This keeps the MVP UI focused on orchestration and coordination while respecting system-of-record boundaries.

---

## 5. Approved artifact-linking standard

### Decision
For MVP, artifacts are linked using a **mixed but standardized reference model**:
- URL-based links are allowed
- repository paths are allowed
- each artifact reference must include a normalized metadata record in cockpit storage

### Minimum artifact metadata
- `artifact_id`
- `artifact_type`
- `artifact_name`
- `artifact_path`
- `linked_entity_type`
- `linked_entity_id`
- optional `artifact_owner`

### Why
This is flexible enough for MVP while still giving the UI a stable way to display and manage links.

---

## 6. Approved reporting refresh pattern

### Decision
The MVP reporting pattern is **scheduled refresh**, not manual export/import and not full real-time architecture.

### Why
Scheduled refresh is practical, stable, and sufficient for leadership and review use cases in MVP.

### Reporting assumption
The cockpit is the operating surface; Power BI is the reporting consumer.

---

## 7. Approved UI implementation readiness statement

The following implementation prerequisites are now considered sufficiently stable for UI build-out:
- Engagement MVP fields are frozen enough
- source-of-truth decisions are made
- first sync assumptions are stable
- direct UI editing scope is defined

This means the team may proceed with:
- UI blueprinting
- API contract definition
- backing-store schema definition
- initial frontend implementation
- integration stub implementation

---

## Summary
For MVP:
- Engagement lives first in the Cockpit app layer
- Account and Opportunity remain Salesforce masters
- BVA remains mastered in the BVA app
- `meddpicc_status` is cockpit-owned in MVP
- the Cockpit UI directly edits engagement and coordination fields
- sync is read-oriented and summary-oriented
- Power BI is reporting-only
- artifacts are linked through normalized references
