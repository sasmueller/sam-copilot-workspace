# System Mapping: Sales Cockpit, Salesforce, Power BI, and Future Implementation Sources

## Purpose
This document maps the logical Sales Cockpit model to current implementation systems:
- Salesforce
- Power BI

It also establishes the design principle that additional implementation sources can be added in the future without redefining the cockpit operating model.

The goal is to ensure the cockpit design is:
- operationally practical
- compatible with CRM realities
- analytically usable
- extensible across future tools and platforms

## Core design principle
The Sales Cockpit is the **logical operating model**.

Implementation systems should be treated as **mappings of that model**, not as the definition of the model.

This means:
- the cockpit defines the business concepts
- implementation systems store, process, or report those concepts
- new systems can be added later through mapping layers
- the logical cockpit model remains stable even if tools change

## Architectural layers

### 1. Logical operating model
This layer defines the business concepts:
- Account
- Engagement
- Opportunity
- BVA
- MEDDPICC Record
- Executive Prep Record
- Portfolio Snapshot
- Artifact Reference

This layer should remain tool-agnostic.

### 2. Canonical data model
This layer defines:
- shared field meanings
- shared statuses
- shared object relationships
- minimum required records

This layer should remain as stable as possible across systems.

### 3. System-specific mappings
This layer maps canonical objects and fields into specific tools.

Current mappings:
- Salesforce
- Power BI

Future mappings may include:
- CPQ platforms
- customer success platforms
- project delivery tools
- planning tools
- data warehouse layers
- custom applications
- other operational systems

## System roles

### Sales Cockpit
Owns:
- logical engagement model
- working process across account, opportunity, BVA, MEDDPICC, and executive prep
- artifact organization and operating memory
- canonical operating semantics

### Salesforce
Owns:
- CRM master data
- account and opportunity transaction records
- stakeholder/contact relationships
- pipeline workflow
- selected structured qualification fields
- references to BVA and related cockpit artifacts where needed

### Power BI
Owns:
- analytical data model
- historical rollups
- cross-object reporting
- executive dashboards
- portfolio trend and inspection views

### Future implementation sources
May own:
- specialized workflow states
- domain-specific calculations
- generated artifacts
- operational details for specific functions

No future tool should redefine the logical cockpit concepts.
It should only map to them.

## Logical object model
The cockpit uses these logical objects:
- Account
- Engagement
- Opportunity
- BVA
- MEDDPICC Record
- Executive Prep Record
- Portfolio Snapshot
- Artifact Reference

These are business operating objects, not assumptions about any single platform.

## Mapping overview

| Logical Object | Salesforce Mapping | Power BI Mapping | Future Tool Mapping | Notes |
|---|---|---|---|---|
| Account | Standard `Account` | `dim_account` | map as needed | Primary customer/account master |
| Opportunity | Standard `Opportunity` | `fact_opportunity` | map as needed | Standard pipeline object |
| Engagement | Custom object recommended | `fact_engagement` | map as needed | Central cockpit object |
| BVA | Custom object recommended | `fact_bva` | map as needed | Business value workflow object |
| MEDDPICC Record | Custom object or Opportunity fields | `fact_meddpicc` | map as needed | Depends on rigor |
| Executive Prep Record | Custom object / Activities / Files | `fact_exec_prep` or support table | map as needed | Depends on structure |
| Portfolio Snapshot | Reporting layer, not transactional object | `fact_portfolio_snapshot` | map as needed | Snapshot-oriented reporting |
| Artifact Reference | Files / Notes / link object | bridge/reference table | map as needed | Durable content linkage |

## Mapping extensibility rule
Whenever a new implementation source is introduced:
1. do not redefine the logical cockpit object
2. do not change object meaning to fit the tool
3. create a system-specific mapping from canonical fields to tool-specific fields
4. define the system-of-record role explicitly
5. define sync direction and ownership clearly

## Salesforce mapping detail

## 1. Account
### Recommended Salesforce object
- Standard `Account`

### Typical field alignment
- `account_id` → Salesforce Account ID
- `account_name` → Account Name
- `industry` → Industry
- `segment` → custom field if needed
- `region` → standard or custom field
- `account_owner` → Account Owner

### Notes
Account should remain the primary customer-level master record.

## 2. Opportunity
### Recommended Salesforce object
- Standard `Opportunity`

### Typical field alignment
- `opportunity_id` → Salesforce Opportunity ID
- `opportunity_name` → Opportunity Name
- `opportunity_stage` → Stage
- `amount` → Amount
- `target_close_date` → Close Date
- `opportunity_owner` → Opportunity Owner
- `forecast_category` → Forecast Category
- `deal_risk_level` → custom field if needed

### Notes
Opportunity should remain the standard commercial pursuit object.

## 3. Engagement
### Recommended Salesforce object
- Custom object: `Engagement__c` or equivalent

### Why custom is recommended
The cockpit uses Engagement as the central operating record, but Salesforce does not provide a standard object that fully covers:
- account-led motions without a live opportunity
- BVA-led motions before a deal is fully formed
- executive-led motions
- simulation scenarios

### Recommended relationships
- `Engagement__c` → lookup to `Account`
- `Engagement__c` → optional lookup to `Opportunity`
- `Engagement__c` → optional lookup to `BVA__c`
- `Engagement__c` → optional lookup to owner/contact references

### Suggested core fields
- Engagement Name
- Engagement Type
- Engagement Owner
- Strategic Objective
- Next Milestone
- Next Action
- BVA Status
- MEDDPICC Status
- Executive Prep Status
- Entry Path
- Overall Status

## 4. BVA
### Recommended Salesforce object
- Custom object: `BVA__c`

### Recommended relationships
- `BVA__c` → lookup to `Account`
- `BVA__c` → optional lookup to `Opportunity`
- `BVA__c` → optional lookup to `Engagement__c`

### Suggested core fields
- BVA Name
- BVA Status
- BVA Phase
- Scenario Type
- Business Problem Summary
- Baseline Metrics Summary
- Value Drivers Summary
- Quantified Outcomes Summary
- Assumptions Summary
- Confidence Level
- Risks and Dependencies Summary
- Executive Summary
- Last Updated Date

### Notes
If the BVA app is the workflow engine, Salesforce does not need to hold the full artifact body.
It should hold enough metadata to:
- find the BVA
- report on BVA progress
- relate it to account and opportunity context

## 5. MEDDPICC Record
### Recommended Salesforce implementation
Two options:

#### Option A: custom object
- `MEDDPICC_Record__c`

#### Option B: fields on Opportunity
Use if you want simple inspection and do not need deep historical detail.

### Recommendation
- Use **Opportunity fields** for lightweight implementation
- Use a **custom object** if you want versioning, inspection history, or multiple qualification checkpoints

## 6. Executive Prep Record
### Recommended Salesforce implementation
#### Lightweight option
Use:
- Activities
- Events
- Tasks
- Files

#### Structured option
Use custom object:
- `Executive_Prep__c`

### Recommendation
If executive prep is a recurring management discipline, prefer a custom object.

## 7. Portfolio Snapshot
### Recommended Salesforce implementation
Do not model this primarily as a transactional Salesforce object.

### Recommendation
Portfolio should mainly be:
- reporting logic
- dashboard views
- analytical outputs

## 8. Stakeholders
### Recommended Salesforce mapping
Use standard CRM relationship objects wherever possible:
- Contact
- Contact Roles
- Account Contact Relationships
- Opportunity Contact Roles

### Notes
Avoid duplicating stakeholder identity too heavily in custom cockpit objects.

## 9. Artifact Reference
### Recommended Salesforce implementation
Use one of these approaches:
- Files with links
- Notes
- custom object for artifact references
- URL fields pointing to GitHub or external storage

### Recommendation
Use structured references, not large duplicated document bodies.

## Power BI mapping detail

## Recommended analytical design
Power BI should model the cockpit as a relational reporting model with:
- dimensions
- facts
- bridge tables
- snapshot logic where needed

## 1. Dimension tables
Recommended dimensions:
- `dim_account`
- `dim_owner`
- `dim_date`
- `dim_stage`
- `dim_engagement_type`
- `dim_status`
- `dim_bva_phase`
- `dim_exec_event_type`
- `dim_priority`

## 2. Fact tables
Recommended facts:
- `fact_engagement`
- `fact_opportunity`
- `fact_bva`
- `fact_meddpicc`
- `fact_exec_prep`
- `fact_portfolio_snapshot`

## 3. Bridge tables
Recommended bridge/reference tables:
- `bridge_engagement_account`
- `bridge_engagement_opportunity`
- `bridge_engagement_stakeholder`
- `bridge_engagement_artifact`
- `bridge_bva_artifact`

## Fact grain recommendations

### `fact_engagement`
One row per engagement.

### `fact_opportunity`
One row per opportunity.

### `fact_bva`
One row per BVA assessment, or one row per BVA snapshot if history is needed.

### `fact_meddpicc`
One row per qualification checkpoint or one row per opportunity current state.

### `fact_exec_prep`
One row per executive prep instance.

### `fact_portfolio_snapshot`
One row per engagement per review period.

## Future implementation source guidance

When a future tool is introduced, document it using the following pattern:

### Tool name
- system purpose
- logical objects it touches
- system-of-record role
- inbound fields
- outbound fields
- sync direction
- artifact ownership
- reporting implications
- overlap risks with existing systems

### Example future tool categories
- CPQ
- customer success platform
- project delivery tool
- planning tool
- data warehouse
- custom internal application

## Recommended system-of-record guidance

| Domain | Primary System of Record | Notes |
|---|---|---|
| Account master | Salesforce | Standard CRM authority |
| Opportunity master | Salesforce | Standard pipeline authority |
| Engagement workflow | Sales Cockpit / custom CRM layer | Depends on implementation |
| BVA workflow state | BVA app | Metadata can sync outward |
| BVA artifacts | Repository and/or app storage | Sync references rather than duplicate bodies |
| MEDDPICC status | Salesforce or cockpit layer | Choose one owner clearly |
| Executive prep artifacts | Repository / CRM links | Depends on structure |
| Portfolio reporting | Power BI | Best home for cross-cutting analysis |

## Key architectural decisions recommended

## Decision 1: Keep Engagement as a logical primary object
Recommendation:
- Yes, keep it in the cockpit model
- Implement it as a custom object where needed

## Decision 2: Keep multi-entry support
Recommendation:
- yes

Why:
- supports BVA-led, opportunity-led, account-led, and simulation-led motions
- avoids forcing a single workflow origin

## Decision 3: Do not replace Opportunity
Recommendation:
- keep Opportunity as the standard commercial CRM object
- relate Engagement to Opportunity when relevant

## Decision 4: Keep canonical semantics outside any one tool
Recommendation:
- yes

Why:
- protects the operating model from tool lock-in
- makes future integration easier
- allows multiple implementation sources over time

## Key risks and ambiguities

### 1. Engagement vs Opportunity overlap
Mitigation:
Define clear operating rules and ownership.

### 2. Duplicate stakeholder data
Mitigation:
Use CRM relationships as identity anchors where possible.

### 3. BVA artifact duplication
Mitigation:
Choose a primary artifact host and sync references only.

### 4. MEDDPICC fragmentation
Mitigation:
Choose one primary operating pattern.

### 5. Tool-driven model drift
Mitigation:
Do not let new implementation tools redefine logical cockpit objects.

## Recommended next step
After approving this mapping approach:
1. refine `cockpit-data-model.md`
2. add system-aware field mappings
3. define which fields are canonical vs tool-local
4. define sync and ownership rules per domain

## Summary
The Sales Cockpit should remain the canonical business operating model.

Salesforce and Power BI are current implementations of that model.
Future implementation sources can be added later through explicit mapping layers.

This preserves:
- business clarity
- tool flexibility
- analytical consistency
- architectural extensibility
