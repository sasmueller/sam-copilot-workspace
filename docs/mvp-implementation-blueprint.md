# MVP Implementation Blueprint

## Purpose
This document defines the minimum viable implementation approach for the Sales Cockpit.

The goal is to move from architecture and data-model design into a practical first implementation that:
- is useful quickly
- avoids overbuilding
- respects system-of-record boundaries
- supports future extensibility

This MVP should prove the cockpit model with the smallest useful set of:
- objects
- fields
- sync flows
- artifacts
- reporting views

## MVP design objective
The MVP should answer one practical question:

**Can the Sales Cockpit coordinate account, opportunity, BVA, and executive prep work in a useful way without requiring the full end-state architecture on day one?**

The answer should come from a lightweight but real implementation.

## MVP principles
- build the minimum central engagement layer first
- integrate with Salesforce rather than replacing it
- treat BVA as a linked workflow, not a required starting point
- use Power BI for a small number of high-value views first
- keep artifact management simple
- prefer manual governance over complex automation in v1 where needed

## Scope of MVP

## In scope
- Engagement as the central cockpit object
- Account and Opportunity linkage
- BVA status linkage
- MEDDPICC summary status
- executive prep summary status
- artifact references
- basic Power BI reporting
- basic sync rules

## Out of scope for MVP
- complex bidirectional writeback
- full MEDDPICC subcomponent automation
- advanced portfolio scoring engines
- full artifact lifecycle automation
- deep contact-role orchestration
- multi-tool future-source onboarding
- complex exception-handling workflows

## MVP architecture

### Systems used in MVP
- Salesforce for account and opportunity master records
- BVA app for BVA workflow state and outputs
- repository for artifacts and documentation
- Power BI for reporting
- cockpit engagement layer as the coordination model

### MVP operating model
The user should be able to:
1. identify an account and optional opportunity
2. create or track an engagement
3. classify whether the motion is account-led, opportunity-led, BVA-led, or executive-led
4. see BVA status
5. see MEDDPICC summary status
6. see executive prep summary status
7. access linked artifacts
8. review a simple portfolio/status dashboard

## MVP object priority

## Priority 1: Engagement
This is the first object to implement.

### Why
Without Engagement, the cockpit has no cross-motion anchor.

### Minimum required fields
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

### Recommended v1 optional fields
- `opportunity_id`
- `source_entry_path`
- `summary`
- `last_updated_date`

## Priority 2: Account and Opportunity linkage
Use Salesforce as the source of truth.

### MVP rule
Do not rebuild account and opportunity management in the cockpit.
Reference and extend them.

### Required linked fields
- `account_id`
- `account_name`
- `opportunity_id` if applicable
- `opportunity_name` if applicable
- `opportunity_stage` if applicable

## Priority 3: BVA linkage
The MVP does not need full BVA synchronization.
It needs useful BVA visibility.

### Minimum BVA-linked fields
- `bva_id`
- `bva_status`
- `bva_phase`
- `bva_last_updated`
- `bva_summary_artifact`

### MVP rule
Detailed BVA model content can remain in the BVA app and linked artifacts.

## Priority 4: MEDDPICC summary
Do not implement full MEDDPICC orchestration first.

### Minimum MEDDPICC fields
- `meddpicc_status`
- optional `meddpicc_summary_artifact`

### MVP rule
Use a summary status first.
Add component-level detail later.

## Priority 5: Executive prep summary
### Minimum fields
- `exec_prep_status`
- `exec_event_type`
- `exec_event_date`
- `exec_brief_artifact`

### MVP rule
Support executive readiness visibility before trying to model every meeting detail.

## Priority 6: Artifact references
### Minimum fields
- `artifact_id`
- `artifact_type`
- `artifact_name`
- `artifact_path`
- `linked_entity_type`
- `linked_entity_id`

### MVP rule
Use links and references, not document duplication.

## MVP workflows

## Workflow 1: Opportunity-led engagement
1. Seller identifies opportunity in Salesforce
2. Cockpit engagement record is created or linked
3. Engagement is associated with account and opportunity
4. Seller updates strategic objective, next milestone, and next action
5. MEDDPICC summary status is tracked
6. If needed, BVA is started and linked
7. Exec prep artifacts are added when relevant

## Workflow 2: BVA-led engagement
1. BVA starts in the BVA app
2. Engagement record is created or linked in cockpit
3. Account is linked
4. Opportunity is linked later if present
5. BVA status and phase are surfaced in cockpit
6. BVA outputs are linked as artifacts
7. Exec readout prep is tracked as needed

## Workflow 3: Account-led / executive-led engagement
1. Strategic account motion is identified
2. Engagement record is created without requiring opportunity or BVA
3. Strategic objective and next action are captured
4. Executive prep status is tracked
5. Opportunity and BVA can be added later

## MVP sync scope

## Sync 1: Salesforce to cockpit
### Required
- account reference data
- opportunity reference data
- opportunity stage
- account owner / opportunity owner if needed

### Recommendation
Start with read-oriented sync into cockpit views rather than complex writeback.

## Sync 2: BVA app to cockpit
### Required
- `bva_id`
- `bva_status`
- `bva_phase`
- `bva_last_updated`
- main artifact or export reference

### Recommendation
Use summary sync rather than full-model sync in MVP.

## Sync 3: Cockpit to Power BI
### Required
- engagement records
- BVA summary fields
- MEDDPICC summary status
- executive prep summary status
- artifact availability indicators

### Recommendation
Favor a simple reporting dataset over real-time complexity.

## Sync 4: Repository references
### Required
- artifact links resolvable from engagement context

### Recommendation
Keep repository integration lightweight in v1.

## MVP ownership rules

| Domain | MVP Owner | Notes |
|---|---|---|
| Account | Salesforce | no cockpit master copy |
| Opportunity | Salesforce | no cockpit master copy |
| Engagement | cockpit layer | central coordination object |
| BVA workflow | BVA app | sync summary only |
| MEDDPICC summary | Salesforce or cockpit, choose one | choose explicitly before build |
| Executive prep summary | cockpit layer | artifact may live in repo |
| Portfolio reporting | Power BI | derived views only |
| Artifact body | repository / source app | reference from cockpit |

## MVP reporting views

## View 1: Engagement list
Purpose:
- show all active engagements

Minimum columns:
- engagement name
- account
- opportunity
- engagement type
- owner
- status
- BVA status
- MEDDPICC status
- exec prep status
- next milestone
- next action

## View 2: BVA coverage view
Purpose:
- show which engagements have or need BVA

Minimum columns:
- engagement name
- account
- engagement type
- BVA required
- BVA status
- BVA phase
- BVA last updated

## View 3: Executive readiness view
Purpose:
- show upcoming executive motions

Minimum columns:
- engagement name
- account
- exec prep status
- exec event type
- exec event date
- brief available

## View 4: Portfolio review summary
Purpose:
- show leadership-level rollup

Minimum metrics:
- active engagements
- BVA in progress
- BVA completed
- engagements at risk
- exec meetings upcoming
- engagements needing leadership attention

## Manual vs automated in MVP

## Keep manual in MVP
- some engagement creation steps
- artifact tagging discipline
- MEDDPICC component detail
- exception handling
- conflict resolution
- portfolio annotation logic

## Automate later
- advanced sync rules
- bidirectional updates
- artifact discovery
- contact-role propagation
- confidence scoring
- portfolio prioritization logic

## MVP implementation decisions to make before build

### Decision 1
Where does the first Engagement object live operationally?
Options:
- custom Salesforce object
- lightweight app/store layer
- repository-backed structured record plus UI layer

### Decision 2
Who owns `meddpicc_status` in MVP?
Choose one:
- Salesforce
- cockpit layer

### Decision 3
How are artifacts linked in MVP?
Choose one:
- repository path links
- URL-based links
- custom artifact reference object
- mixed approach with standards

### Decision 4
What is the first reporting refresh pattern?
Choose one:
- manual export/import
- scheduled refresh
- direct connected reporting model

## Recommended MVP sequence

## Phase 1: Foundation
- finalize Engagement MVP fields
- choose operational home for Engagement
- choose MEDDPICC summary owner
- define artifact-linking convention

## Phase 2: First integration
- connect account and opportunity context
- add BVA summary linkage
- add executive prep summary fields

## Phase 3: First reporting
- build engagement list
- build BVA coverage view
- build executive readiness view
- build simple portfolio review summary

## Phase 4: Governance hardening
- validate ownership rules
- validate sync behavior
- refine required fields
- clean up duplicate artifact patterns

## MVP success criteria

### Success means:
- users can track engagements across different entry paths
- engagement records remain useful without requiring immediate BVA creation
- BVA progress is visible when relevant
- executive prep readiness is visible
- leadership can review a simple portfolio view
- systems do not fight over ownership of core fields

### MVP does not require:
- full automation
- full workflow coverage
- perfect data completeness
- every future integration

## Recommended implementation posture
For MVP, optimize for:
- clarity
- usefulness
- discipline
- extensibility

Do not optimize first for:
- maximum automation
- full platform generalization
- edge-case completeness

## Summary
The MVP should introduce Engagement as the central cockpit object and connect it lightly to Salesforce, the BVA app, repository artifacts, and Power BI.

The MVP should prove:
- multi-entry cockpit value
- BVA-linked but not BVA-dependent workflow
- practical ownership boundaries
- useful leadership visibility

That is enough to validate the model before deeper automation and broader system integration.
