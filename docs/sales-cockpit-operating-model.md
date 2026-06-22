# Sales Cockpit Operating Model

## Purpose
The Sales Cockpit is the operating layer for managing strategic account work, opportunities, business value assessments, executive preparation, qualification rigor, and portfolio visibility.

The cockpit is designed to be valuable even when a customer engagement does **not** begin in the Business Value Assessment (BVA) app.

## Design principle
The cockpit is **BVA-centered but not BVA-dependent**.

This means:
- the BVA app is a central pillar for value discovery, quantification, and executive output
- the cockpit must still work when an engagement starts elsewhere
- every engagement should have a cockpit record whether or not a BVA exists yet
- BVA should enrich the cockpit, not gate it

## Entry paths
An engagement can enter the cockpit through multiple starting points.

### 1. BVA-led
Use this path when the engagement starts with a value assessment motion.

Typical triggers:
- customer asks for business case support
- seller wants to frame transformation value early
- executive conversation needs quantified outcomes

### 2. Opportunity-led
Use this path when the engagement begins as a live sales pursuit.

Typical triggers:
- new deal identified
- opportunity review begins
- qualification starts before value assessment

### 3. Account-led / Executive-led
Use this path when the motion starts from strategic account planning or executive engagement.

Typical triggers:
- account planning session
- executive sponsor prep
- portfolio prioritization
- whitespace or strategic growth planning

## Core operating rule
Every engagement should have a cockpit record first.

That record can later be enriched by:
- a BVA
- a MEDDPICC scorecard
- executive briefing material
- portfolio review updates

## Core engagement record
Each engagement should maintain a common set of fields, regardless of entry point.

## Required fields
- Account name
- Opportunity name or initiative name
- Engagement type
- Owner
- Stage
- Strategic objective
- Key stakeholders
- Next milestone
- Next action
- BVA status
- MEDDPICC status
- Executive prep status
- Linked artifacts

## Suggested statuses
### BVA status
- Not needed yet
- Candidate
- In progress
- Complete
- Validated
- Presented

### MEDDPICC status
- Not started
- Draft
- In review
- Active
- Complete

### Executive prep status
- Not planned
- Planned
- Drafting
- Ready
- Delivered

## Role of the BVA app
The BVA app is a major engine within the cockpit, but it is not the only way to start or manage work.

The BVA app should support:
- structured value discovery
- baseline assessment
- value identification
- quantification
- business value mapping
- executive readout generation
- simulation scenarios

## What the BVA app should provide to the cockpit
When a BVA exists, it should contribute structured outputs to the cockpit.

### Recommended BVA outputs
- account context
- initiative or opportunity context
- business problem summary
- baseline metrics
- value drivers
- quantified outcomes
- assumptions
- confidence level
- risks and dependencies
- executive summary
- business value map
- follow-up actions

## What the cockpit should own outside the BVA app
The cockpit should remain useful even without a BVA.

### Cockpit-owned capabilities
- account strategy and stakeholder context
- opportunity tracking and pursuit planning
- MEDDPICC inspection and qualification rigor
- executive prep and briefing orchestration
- portfolio rollups and prioritization
- repository of templates, examples, and historical outputs

## Recommended repository responsibilities
### `accounts/`
Long-lived account context, plans, stakeholder maps, and strategic notes.

### `opportunities/`
Active pursuit records, next steps, and opportunity-specific planning.

### `bva/`
Templates, examples, exports, drafts, and links to app-generated BVA artifacts.

### `meddpicc/`
Qualification scorecards, inspection notes, and deal rigor artifacts.

### `exec-prep/`
Executive briefing documents, key messages, and meeting preparation materials.

### `portfolio/`
Rollups, prioritization views, weekly reviews, and leadership summaries.

## Working model
The recommended model is:
- app = workflow engine
- repository = system of record and operating memory

This split allows:
- app-led creation of structured BVA outputs
- opportunity-led motions without immediate BVA dependency
- simulation and rehearsal workflows before real customer use
- persistent historical record across engagements

## Recommended engagement lifecycle
1. Create or identify the engagement in the cockpit
2. Capture account and opportunity context
3. Decide whether a BVA is needed now, later, or not yet
4. If needed, run the BVA app and attach outputs
5. Update MEDDPICC and executive prep as the motion evolves
6. Roll engagement status into portfolio review

## Decision rule for invoking BVA
Use BVA when one or more of the following are true:
- the customer needs a quantified business case
- internal stakeholders need value justification
- executive messaging requires outcome framing
- the deal needs stronger differentiation through value articulation
- investment tradeoffs need structured analysis

## Architectural guideline
The cockpit must support:
- BVA-led motions
- opportunity-led motions
- account-led motions
- simulation-led rehearsal

No single entry point should be mandatory.

## Summary
The Sales Cockpit should function as a multi-entry operating system for strategic selling.

The BVA app is a central pillar because it strengthens discovery, value articulation, and executive communication.

However, the cockpit must remain valuable even when a motion begins outside the BVA workflow.
