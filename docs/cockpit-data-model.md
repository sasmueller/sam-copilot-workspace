# Cockpit Data Model

## Purpose
This document defines the canonical shared data model for the Sales Cockpit.

The goal is to ensure that:
- engagements can start from multiple entry points
- the cockpit remains useful with or without a BVA
- data stays consistent across Salesforce, Power BI, and future implementation sources
- system mappings do not redefine the business meaning of cockpit objects

This is a **canonical operating model**, not a tool-specific schema.

## Relationship to system mappings
This document defines:
- logical objects
- canonical field meanings
- required vs optional fields
- statuses and enumerations
- shared object relationships

System-specific object and field mappings are defined separately in:
- `docs/system-mapping-salesforce-powerbi.md`

## Design principles
- The Sales Cockpit is the operating layer
- The canonical model should be tool-agnostic
- System-specific mappings should adapt to the canonical model
- Every engagement should have a common record
- BVA is a central but non-mandatory linked object
- Canonical semantics should remain stable even if implementation tools change

## Canonical object model
The cockpit uses these canonical objects:
- Account
- Engagement
- Opportunity
- BVA
- MEDDPICC Record
- Executive Prep Record
- Portfolio Snapshot
- Artifact Reference

Not every engagement requires every object at the start.

## Field classification model
Each field should be understood in four ways:
- **Canonical**: part of the logical cockpit model
- **System of record**: primary owner of the field value
- **Implementation mapping**: where the field typically maps in Salesforce / Power BI
- **Future extensibility**: whether additional tool mappings may be added later

## 1. Account object

### Purpose
Represents the customer or target account context.

### Core fields

| Canonical Field | Required | Description | Primary System of Record | Salesforce Mapping | Power BI Mapping | Future Tool Mapping |
|---|---|---|---|---|---|---|
| `account_id` | Yes | Unique account identifier | Salesforce | Account ID | `dim_account.account_id` | As needed |
| `account_name` | Yes | Account name | Salesforce | Account Name | `dim_account.account_name` | As needed |
| `industry` | No | Industry classification | Salesforce | Industry | `dim_account.industry` | As needed |
| `segment` | No | Market segment | Salesforce | standard or custom field | `dim_account.segment` | As needed |
| `region` | No | Geographic region | Salesforce | standard or custom field | `dim_account.region` | As needed |
| `account_owner` | No | Account owner | Salesforce | Account Owner | `dim_account.owner_key` | As needed |

## 2. Engagement object

### Purpose
Represents the central operating record for a sales motion, regardless of entry point.

### Notes
This is the canonical anchor of the cockpit.
It may map to a custom object in implementation systems.

### Core fields

| Canonical Field | Required | Description | Primary System of Record | Salesforce Mapping | Power BI Mapping | Future Tool Mapping |
|---|---|---|---|---|---|---|
| `engagement_id` | Yes | Unique engagement identifier | Cockpit layer | custom object ID | `fact_engagement.engagement_id` | As needed |
| `engagement_name` | Yes | Human-readable engagement name | Cockpit layer | Engagement Name | `fact_engagement.engagement_name` | As needed |
| `engagement_type` | Yes | Type of entry motion | Cockpit layer | custom field | `fact_engagement.engagement_type_key` | As needed |
| `engagement_owner` | Yes | Person responsible for the engagement | Cockpit layer / Salesforce | custom owner field | `fact_engagement.owner_key` | As needed |
| `account_id` | Yes | Linked account identifier | Salesforce | lookup to Account | `fact_engagement.account_id` | As needed |
| `strategic_objective` | Yes | Main business or sales objective | Cockpit layer | custom field | `fact_engagement.strategic_objective` | As needed |
| `next_milestone` | Yes | Next major checkpoint | Cockpit layer | custom field | `fact_engagement.next_milestone` | As needed |
| `next_action` | Yes | Immediate next action | Cockpit layer | custom field | `fact_engagement.next_action` | As needed |
| `status` | Yes | Overall engagement status | Cockpit layer | custom status field | `fact_engagement.status_key` | As needed |
| `source_entry_path` | No | How the engagement entered the cockpit | Cockpit layer | custom field | `fact_engagement.entry_path_key` | As needed |
| `created_date` | No | Creation date | source system | CreatedDate | `fact_engagement.created_date_key` | As needed |
| `last_updated_date` | No | Last update timestamp | source system | LastModifiedDate | `fact_engagement.updated_date_key` | As needed |
| `summary` | No | Short narrative summary | Cockpit layer | custom text field | `fact_engagement.summary` | As needed |

### Enumerations

#### `engagement_type`
- `account-led`
- `opportunity-led`
- `bva-led`
- `executive-led`
- `simulation`

#### `source_entry_path`
- `account`
- `opportunity`
- `bva`
- `executive`
- `portfolio`
- `simulation`

#### `status`
- `not-started`
- `active`
- `on-hold`
- `complete`
- `archived`

## 3. Opportunity object

### Purpose
Represents the commercial opportunity when one exists.

### Core fields

| Canonical Field | Required | Description | Primary System of Record | Salesforce Mapping | Power BI Mapping | Future Tool Mapping |
|---|---|---|---|---|---|---|
| `opportunity_id` | No | Unique opportunity identifier | Salesforce | Opportunity ID | `fact_opportunity.opportunity_id` | As needed |
| `opportunity_name` | No | Opportunity name | Salesforce | Opportunity Name | `fact_opportunity.opportunity_name` | As needed |
| `engagement_id` | No | Linked engagement identifier | Cockpit layer | lookup from Engagement | `fact_opportunity.engagement_id` | As needed |
| `opportunity_stage` | No | Pipeline stage | Salesforce | Stage | `fact_opportunity.stage_key` | As needed |
| `amount` | No | Commercial amount | Salesforce | Amount | `fact_opportunity.amount` | As needed |
| `target_close_date` | No | Expected close date | Salesforce | Close Date | `fact_opportunity.close_date_key` | As needed |
| `forecast_category` | No | Forecast classification | Salesforce | Forecast Category | `fact_opportunity.forecast_category` | As needed |
| `deal_risk_level` | No | Risk assessment | Salesforce / cockpit layer | custom field | `fact_opportunity.risk_key` | As needed |
| `opportunity_owner` | No | Opportunity owner | Salesforce | Opportunity Owner | `fact_opportunity.owner_key` | As needed |

### Enumerations

#### `deal_risk_level`
- `low`
- `medium`
- `high`
- `critical`

## 4. Stakeholder fields

### Purpose
Represents key human roles associated with the engagement.

### Notes
Identity should preferably come from CRM relationship structures, while cockpit records reference those roles.

| Canonical Field | Required | Description | Primary System of Record | Salesforce Mapping | Power BI Mapping | Future Tool Mapping |
|---|---|---|---|---|---|---|
| `primary_sponsor` | No | Main sponsor | Salesforce | Contact / relationship mapping | bridge or support table | As needed |
| `economic_buyer` | No | Economic buyer | Salesforce | Contact role / custom mapping | bridge or support table | As needed |
| `champion` | No | Internal champion | Salesforce / cockpit layer | Contact role / custom mapping | bridge or support table | As needed |
| `technical_stakeholder` | No | Technical stakeholder | Salesforce | Contact role / custom mapping | bridge or support table | As needed |
| `executive_stakeholder` | No | Executive stakeholder | Salesforce | Contact role / custom mapping | bridge or support table | As needed |
| `stakeholder_notes` | No | Stakeholder context notes | Cockpit layer | custom note field / linked artifact | support table | As needed |

## 5. BVA object

### Purpose
Represents the Business Value Assessment linked to an engagement.

### Notes
A BVA may or may not exist for every engagement.

### Core fields

| Canonical Field | Required | Description | Primary System of Record | Salesforce Mapping | Power BI Mapping | Future Tool Mapping |
|---|---|---|---|---|---|---|
| `bva_id` | No | Unique BVA identifier | BVA app / cockpit layer | `BVA__c` ID | `fact_bva.bva_id` | As needed |
| `engagement_id` | No | Linked engagement identifier | Cockpit layer | lookup to Engagement | `fact_bva.engagement_id` | As needed |
| `bva_status` | Yes | Current BVA lifecycle status | BVA app / cockpit layer | BVA Status | `fact_engagement.bva_status_key` and/or `fact_bva.status_key` | As needed |
| `bva_required` | No | Whether BVA is needed | Cockpit layer | custom field | `fact_engagement.bva_required_flag` | As needed |
| `bva_phase` | No | Current BVA phase | BVA app | BVA Phase | `fact_bva.phase_key` | As needed |
| `bva_scenario_type` | No | Real or simulation context | BVA app | Scenario Type | `fact_bva.scenario_type_key` | As needed |
| `bva_last_updated` | No | Last BVA update timestamp | BVA app | Last Updated Date | `fact_bva.updated_date_key` | As needed |
| `business_problem_summary` | No | Summary of business problem | BVA app | custom text field | `fact_bva.business_problem_summary` | As needed |
| `baseline_metrics_summary` | No | Summary of baseline metrics | BVA app | custom text field | `fact_bva.baseline_metrics_summary` | As needed |
| `value_drivers_summary` | No | Summary of value drivers | BVA app | custom text field | `fact_bva.value_drivers_summary` | As needed |
| `quantified_outcomes_summary` | No | Summary of quantified outcomes | BVA app | custom text field | `fact_bva.quantified_outcomes_summary` | As needed |
| `assumptions_summary` | No | Summary of assumptions | BVA app | custom text field | `fact_bva.assumptions_summary` | As needed |
| `confidence_level` | No | Confidence in the value model | BVA app / cockpit layer | custom field | `fact_bva.confidence_key` | As needed |
| `risks_dependencies_summary` | No | Key risks and dependencies | BVA app | custom text field | `fact_bva.risks_dependencies_summary` | As needed |
| `executive_summary` | No | Executive-facing summary | BVA app | custom text field / linked artifact | `fact_bva.executive_summary` | As needed |
| `bva_summary_artifact` | No | Link to primary BVA artifact | Repository / app storage | URL / file reference | bridge table | As needed |

### Enumerations

#### `bva_required`
- `yes`
- `no`
- `undecided`

#### `bva_status`
- `not-needed-yet`
- `candidate`
- `in-progress`
- `complete`
- `validated`
- `presented`

#### `bva_phase`
- `customer-setup`
- `discovery`
- `baseline-assessment`
- `value-identification`
- `quantification`
- `business-value-map`
- `executive-readout`

#### `bva_scenario_type`
- `real-customer`
- `simulation`
- `template`

#### `confidence_level`
- `low`
- `medium`
- `high`

## 6. MEDDPICC Record

### Purpose
Represents qualification state and inspection rigor.

| Canonical Field | Required | Description | Primary System of Record | Salesforce Mapping | Power BI Mapping | Future Tool Mapping |
|---|---|---|---|---|---|---|
| `meddpicc_status` | Yes | Overall MEDDPICC state | Salesforce / cockpit layer | custom object or Opportunity fields | `fact_meddpicc.status_key` and/or `fact_engagement.meddpicc_status_key` | As needed |
| `metrics_status` | No | Metrics qualification status | Salesforce / cockpit layer | custom field | `fact_meddpicc.metrics_status_key` | As needed |
| `economic_buyer_status` | No | Economic buyer qualification status | Salesforce / cockpit layer | custom field | `fact_meddpicc.economic_buyer_status_key` | As needed |
| `decision_criteria_status` | No | Decision criteria clarity | Salesforce / cockpit layer | custom field | `fact_meddpicc.decision_criteria_status_key` | As needed |
| `decision_process_status` | No | Decision process clarity | Salesforce / cockpit layer | custom field | `fact_meddpicc.decision_process_status_key` | As needed |
| `paper_process_status` | No | Paper process clarity | Salesforce / cockpit layer | custom field | `fact_meddpicc.paper_process_status_key` | As needed |
| `identify_pain_status` | No | Pain qualification status | Salesforce / cockpit layer | custom field | `fact_meddpicc.identify_pain_status_key` | As needed |
| `champion_status` | No | Champion strength | Salesforce / cockpit layer | custom field | `fact_meddpicc.champion_status_key` | As needed |
| `competition_status` | No | Competitive position status | Salesforce / cockpit layer | custom field | `fact_meddpicc.competition_status_key` | As needed |
| `meddpicc_summary_artifact` | No | Linked qualification artifact | Repository / CRM link | file / URL / custom object | bridge table | As needed |

### Enumerations

#### `meddpicc_status`
- `not-started`
- `draft`
- `in-review`
- `active`
- `complete`

#### component status values
- `unknown`
- `partial`
- `validated`

## 7. Executive Prep Record

### Purpose
Represents preparation for executive interactions and readouts.

| Canonical Field | Required | Description | Primary System of Record | Salesforce Mapping | Power BI Mapping | Future Tool Mapping |
|---|---|---|---|---|---|---|
| `exec_prep_status` | Yes | Current executive prep state | Cockpit layer / Salesforce | custom object or activity model | `fact_exec_prep.status_key` and/or `fact_engagement.exec_prep_status_key` | As needed |
| `exec_event_type` | No | Type of executive interaction | Cockpit layer / Salesforce | custom field | `fact_exec_prep.event_type_key` | As needed |
| `exec_event_date` | No | Date of event | Salesforce / cockpit layer | Event Date | `fact_exec_prep.event_date_key` | As needed |
| `exec_objective` | No | Purpose of the engagement | Cockpit layer | custom field | `fact_exec_prep.exec_objective` | As needed |
| `exec_key_message` | No | Primary executive message | Cockpit layer | custom field | `fact_exec_prep.exec_key_message` | As needed |
| `exec_brief_artifact` | No | Linked executive brief | Repository / CRM link | file / URL | bridge table | As needed |
| `exec_readout_artifact` | No | Linked readout | Repository / CRM link | file / URL | bridge table | As needed |

### Enumerations

#### `exec_prep_status`
- `not-planned`
- `planned`
- `drafting`
- `ready`
- `delivered`

#### `exec_event_type`
- `customer-meeting`
- `qbr`
- `ebr`
- `internal-review`
- `board-level`
- `other`

## 8. Portfolio Snapshot

### Purpose
Represents portfolio-level prioritization and review state.

### Notes
This is primarily analytical and review-oriented rather than a transactional object.

| Canonical Field | Required | Description | Primary System of Record | Salesforce Mapping | Power BI Mapping | Future Tool Mapping |
|---|---|---|---|---|---|---|
| `portfolio_priority` | No | Relative priority | Cockpit layer / reporting layer | custom field or derived field | `fact_portfolio_snapshot.priority_key` | As needed |
| `portfolio_theme` | No | Strategic grouping theme | Cockpit layer / reporting layer | custom field or derived field | `fact_portfolio_snapshot.theme` | As needed |
| `portfolio_risk_flag` | No | Review risk flag | Cockpit layer / reporting layer | custom field or derived field | `fact_portfolio_snapshot.risk_flag_key` | As needed |
| `leadership_attention_required` | No | Whether leadership attention is needed | Cockpit layer / reporting layer | custom field or derived field | `fact_portfolio_snapshot.leadership_attention_flag` | As needed |
| `portfolio_notes` | No | Portfolio review notes | Cockpit layer / reporting layer | note field / external artifact | `fact_portfolio_snapshot.notes` | As needed |

### Enumerations

#### `portfolio_priority`
- `p1`
- `p2`
- `p3`

#### `portfolio_risk_flag`
- `none`
- `watch`
- `elevated`
- `critical`

#### `leadership_attention_required`
- `yes`
- `no`

## 9. Artifact Reference

### Purpose
Represents durable links to working documents and outputs.

| Canonical Field | Required | Description | Primary System of Record | Salesforce Mapping | Power BI Mapping | Future Tool Mapping |
|---|---|---|---|---|---|---|
| `artifact_id` | No | Unique artifact identifier | Repository / source system | custom reference / file ID | bridge table | As needed |
| `artifact_type` | No | Type of artifact | Cockpit layer | custom field | bridge or support table | As needed |
| `artifact_name` | No | Human-readable artifact name | source system | file title / custom field | bridge or support table | As needed |
| `artifact_path` | No | Path or URL to artifact | source system | URL / file link | bridge table | As needed |
| `artifact_owner` | No | Owner of artifact | source system | owner / creator | bridge or support table | As needed |
| `artifact_status` | No | Current artifact state | source system / cockpit layer | custom field | bridge or support table | As needed |
| `linked_entity_type` | No | What object this artifact supports | Cockpit layer | custom field | bridge table | As needed |
| `linked_entity_id` | No | Linked object identifier | Cockpit layer | lookup / reference ID | bridge table | As needed |

### Enumerations

#### `artifact_type`
- `account-plan`
- `opportunity-brief`
- `bva-discovery`
- `bva-model`
- `bva-executive-summary`
- `meddpicc-scorecard`
- `exec-brief`
- `portfolio-review`
- `stakeholder-map`
- `other`

#### `artifact_status`
- `draft`
- `active`
- `final`

#### `linked_entity_type`
- `account`
- `engagement`
- `opportunity`
- `bva`
- `exec-prep`
- `portfolio`

## 10. Minimum viable cockpit record

At minimum, every engagement record should contain:

| Canonical Field | Required |
|---|---|
| `engagement_id` | Yes |
| `engagement_name` | Yes |
| `engagement_type` | Yes |
| `engagement_owner` | Yes |
| `account_id` | Yes |
| `strategic_objective` | Yes |
| `next_milestone` | Yes |
| `next_action` | Yes |
| `status` | Yes |
| `bva_status` | Yes |
| `meddpicc_status` | Yes |
| `exec_prep_status` | Yes |

This is the minimum viable cockpit record.

## 11. Canonical governance rules

### Rule 1
Logical object meanings must not be redefined by implementation systems.

### Rule 2
Canonical statuses should remain stable even if tools change.

### Rule 3
When a new tool is introduced, map it to canonical fields rather than rewriting the canonical model around the tool.

### Rule 4
Choose one primary system of record per field domain wherever possible.

### Rule 5
Store references to artifacts when practical, rather than duplicating large document bodies across tools.

## Summary
The cockpit data model is the canonical business model for multi-entry sales motions.

It supports:
- BVA-led motions
- opportunity-led motions
- account-led motions
- executive-led motions
- simulation scenarios

The engagement record is the canonical working anchor.
System implementations such as Salesforce, Power BI, and future tools should map to this model rather than redefine it.
