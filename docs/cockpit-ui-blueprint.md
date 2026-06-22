# Cockpit UI Blueprint

## Purpose
This document defines the MVP UI blueprint for the Sales Cockpit web application.

It translates the architecture, data model, sync model, and MVP decisions into an implementable user interface design.

This document assumes the decisions in:
- `docs/mvp-decisions.md`
- `docs/mvp-implementation-blueprint.md`
- `docs/cockpit-data-model.md`
- `docs/sync-and-ownership-rules.md`

## UI role in MVP
The Cockpit web app is the **operating workspace** for engagement coordination.

It is not intended in MVP to replace:
- Salesforce for CRM master editing
- the BVA app for detailed BVA authoring
- Power BI for advanced reporting

Instead, the UI should:
- provide a central engagement workspace
- expose cross-system context in one place
- allow direct editing of cockpit-owned workflow fields
- link users into source systems and artifacts when deeper work is needed

## Visual design direction
The Cockpit should feel like a **state-of-the-art premium web product**, not a generic internal tool.

The intended direction is:
- Apple-like in clarity, restraint, polish, and visual confidence
- modern, calm, and content-first
- elegant rather than busy
- premium without being ornamental
- highly usable for enterprise workflows

This does **not** mean copying Apple branding.
It means applying high-end product design principles to the cockpit experience.

## MVP UI goals
The UI must enable users to:
1. create an engagement
2. view and edit engagement workflow state
3. inspect linked account and opportunity context
4. see BVA summary progress
5. see MEDDPICC summary status
6. see executive prep readiness
7. access artifacts and source systems
8. review a simple portfolio of active engagements

## Personas

### Seller
Needs to:
- manage active motions
- track next actions
- understand BVA and executive readiness state

### Value engineer / strategist
Needs to:
- see where BVA exists or is needed
- follow linked BVA outputs
- coordinate with engagement owners

### Manager
Needs to:
- review active engagements
- inspect status and next milestones
- identify blocked or underdeveloped motions

### Leader
Needs to:
- review portfolio health
- identify high-priority or at-risk engagements
- inspect executive-readiness patterns

## Experience principles

### Principle 1: clarity over density
The interface should prioritize readability, hierarchy, and focus over maximum information density.

### Principle 2: premium restraint
Use fewer UI elements, but make them better designed and more intentional.

### Principle 3: summary first
Show essential status and next-step information before deeper detail.

### Principle 4: cross-system clarity
If a field is sourced from Salesforce or the BVA app, make that visible in the UI.

### Principle 5: link out, do not duplicate
If deep work belongs elsewhere, provide a clear link instead of rebuilding the whole tool.

### Principle 6: low-friction updates
The engagement owner should be able to update next action, milestone, and summary quickly.

### Principle 7: polished interaction
Transitions, loading states, empty states, and feedback should feel smooth and intentional.

## Page map

## 1. Engagement list page
Primary purpose:
- default working list for active engagements

### Capabilities
- list active engagements
- filter by owner
- filter by engagement type
- filter by status
- filter by BVA status
- filter by executive prep status
- search by engagement name, account, or opportunity
- open engagement detail page
- create a new engagement

### Minimum columns
- engagement name
- account name
- opportunity name
- engagement owner
- engagement type
- status
- BVA status
- MEDDPICC status
- exec prep status
- next milestone
- next action
- last updated date if available

## 2. Engagement detail page
Primary purpose:
- serve as the core operating workspace

### Layout sections

#### A. Header section
Show:
- engagement name
- engagement owner
- engagement type
- status
- account name
- linked opportunity name if present
- quick links to Salesforce account/opportunity

#### B. Strategic overview section
Editable fields:
- strategic objective
- next milestone
- next action
- summary

#### C. BVA section
Read-only summary fields:
- BVA status
- BVA phase
- BVA last updated

Actions:
- open BVA artifact
- open BVA source app if link exists

#### D. MEDDPICC section
Editable field:
- `meddpicc_status`

Optional display:
- scorecard artifact link if present

#### E. Executive prep section
Editable fields:
- `exec_prep_status`
- `exec_event_type`
- `exec_event_date`

Actions:
- open brief artifact
- open readout artifact if present

#### F. Artifacts section
Show linked artifacts with:
- artifact type
- artifact name
- open link action
- optional owner

Allow:
- add artifact reference
- remove artifact reference

#### G. Context section
Read-only fields from Salesforce where available:
- account name
- industry
- segment
- region
- opportunity name
- opportunity stage
- opportunity owner

## 3. Create engagement page / modal
Primary purpose:
- create a new cockpit engagement quickly

### Required fields
- account selector
- engagement name
- engagement type
- engagement owner
- strategic objective
- next milestone
- next action
- status

### Optional fields
- opportunity selector
- summary
- source entry path

### Post-create behavior
After creation, navigate directly to engagement detail.

## 4. Portfolio page
Primary purpose:
- lightweight leadership and manager review view

### MVP scope
This page can be native in the web app, Power BI-embedded later, or link out initially.

### Minimum content
- active engagement count
- engagements by type
- engagements by status
- BVA in-progress / complete counts
- upcoming exec interactions
- list of engagements needing attention

## Navigation model

### Global navigation
- Engagements
- Portfolio
- Create Engagement

### Secondary navigation
Within engagement detail:
- Overview
- BVA
- MEDDPICC
- Executive Prep
- Artifacts
- Context

If secondary nav is too heavy for MVP, use a single scroll page with sections.

## UI field behavior rules

## Editable in UI
The following are directly editable in the Cockpit UI:
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
- artifact references

## Read-only in UI
The following are read-only in the Cockpit UI in MVP:
- account master fields
- opportunity master fields
- BVA-owned summary fields
- BVA detailed content
- reporting-derived portfolio metrics

## Field styling guidance
- editable fields should look clearly editable
- read-only fields from source systems should be visibly distinct
- linked external-system fields should have open-link affordances
- status fields should use consistent badges/colors
- whitespace and layout should support a premium, uncluttered feel
- avoid heavy gridlines and overly boxed enterprise styling

## MVP status visualization

### Recommended badge approach
Use consistent badge styles for:
- engagement status
- BVA status
- MEDDPICC status
- exec prep status

### Suggested visual treatment
- green = healthy / complete / ready
- amber = in progress / watch
- red = blocked / critical if introduced later
- gray = not started / unknown / not applicable

## Suggested MVP components

### Reusable components
- status badge
- field/value row
- editable text area
- editable select
- artifact list
- linked context card
- summary panel
- create form modal or page

### Core layout components
- page header
- filter bar
- engagement table
- detail section cards
- action toolbar

## Integration display rules

### Salesforce integration
Display:
- account context
- opportunity context
- open-in-Salesforce links

Do not allow CRM master editing in MVP.

### BVA integration
Display:
- BVA status
- BVA phase
- BVA last updated
- open BVA artifact / app link

Do not allow detailed BVA authoring in MVP.

### Power BI integration
Use as:
- linked reporting destination
- or later embedded dashboard content

Do not use it as the operational editing surface.

## API/UI contract guidance

### UI should request normalized engagement view models
The backend should provide a normalized record that already resolves:
- engagement fields
- account display fields
- opportunity display fields
- BVA summary fields
- artifact references

This reduces frontend complexity.

### Suggested top-level UI view model
- engagement core
- CRM context
- BVA summary
- MEDDPICC summary
- exec prep summary
- artifact list
- external links

## MVP implementation recommendation

### Recommended build order
1. engagement list page
2. create engagement flow
3. engagement detail page
4. artifact linking
5. portfolio view
6. polish and role-based refinements

### Why this order
It enables actual workflow usage as early as possible.

## Non-goals for MVP UI
- advanced workflow automation
- embedded BVA modeling engine
- full CRM editing surface
- deep permissions model beyond basic role handling
- advanced admin tooling
- complex audit history UI

## Success criteria
The MVP UI is successful if users can:
- create engagements quickly
- maintain strategic objective, next milestone, and next action
- see BVA and executive readiness at a glance
- navigate to source systems and artifacts without confusion
- review active work in one operating surface

## Summary
The Cockpit web app should be implemented as the MVP operating workspace once the decision set is stable.

Its role is to:
- own engagement coordination
- expose cross-system context
- allow editing of cockpit-owned fields
- keep deep system-specific work in the right source systems

This provides a practical UI foundation without overcommitting the architecture too early.
