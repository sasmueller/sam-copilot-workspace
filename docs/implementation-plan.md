# Implementation Plan

## Purpose
This document defines the practical implementation plan for delivering the MVP Sales Cockpit web application.

It translates the architecture and specifications into build phases, workstreams, and sequencing.

This plan assumes:
- MVP decisions are approved
- UI blueprint is stable
- API contract is ready for implementation
- backend schema is accepted for MVP
- premium visual direction is a real delivery requirement, not a later polish item

## Implementation goals
Deliver an MVP that can:
- create and manage engagements
- display linked CRM and BVA context
- manage artifact references
- provide a simple portfolio summary
- respect source-of-truth boundaries
- present a premium state-of-the-art UI experience

## Workstreams
Implementation is organized into four parallel workstreams:
- backend
- frontend
- integrations
- reporting

## Delivery phases

## Phase 0: Setup and scaffolding
Goal:
- create the project foundation

### Tasks
- choose frontend framework
- choose backend framework
- choose database
- initialize repository/app structure
- establish environment configuration
- define auth placeholder strategy
- create CI basics
- create linting and formatting setup
- define design token approach
- choose UI component/styling strategy

### Outputs
- runnable app skeleton
- deployable development environment
- basic CI validation
- initial design system foundation

## Phase 1: Backend MVP foundation
Goal:
- implement cockpit-owned core APIs and persistence

### Tasks
- create database migrations for:
  - `engagements`
  - `engagement_external_refs`
  - `engagement_artifacts`
- implement engagement domain model
- implement create engagement endpoint
- implement list engagements endpoint
- implement get engagement detail endpoint
- implement patch engagement endpoint
- implement artifact create/delete endpoints
- implement validation and error model
- implement normalized read-model composition

### Outputs
- stable local API for core cockpit workflows

## Phase 2: Frontend MVP foundation
Goal:
- implement the first usable cockpit UI

### Tasks
- implement app shell and routes
- implement design tokens for:
  - typography
  - spacing
  - colors
  - radius
  - shadows
  - motion
- implement engagement list page
- implement create engagement page
- implement engagement detail page
- implement artifact reference management UI
- implement portfolio page
- implement reusable components:
  - status badge
  - detail cards
  - editable fields
  - table
  - loading/error states
- implement polished empty/loading/error states

### Outputs
- usable end-to-end cockpit UI against local/mock or real API
- premium visual baseline established early

## Phase 3: First integrations
Goal:
- connect external read-only context

### Tasks
- integrate Salesforce account context retrieval
- integrate Salesforce opportunity context retrieval
- integrate BVA summary retrieval
- resolve external system links for:
  - Salesforce account
  - Salesforce opportunity
  - BVA source/artifact
- populate `engagement_external_refs`
- define sync/update job or integration flow

### Outputs
- normalized engagement detail with real cross-system context

## Phase 4: Reporting MVP
Goal:
- provide first portfolio and reporting visibility

### Tasks
- implement portfolio summary backend logic
- expose `GET /api/v1/portfolio/summary`
- implement frontend portfolio page
- define Power BI feed/export dataset shape
- enable scheduled refresh flow for reporting consumer

### Outputs
- first leadership-ready portfolio summary
- first reporting-compatible data feed

## Phase 5: Stabilization and governance
Goal:
- harden the MVP for real usage

### Tasks
- test end-to-end workflows
- validate source-of-truth boundaries
- verify UI editing constraints
- verify artifact link behavior
- verify integration failure handling
- clean up error handling and messaging
- refine required fields if necessary
- perform UX polish pass for spacing, hierarchy, and motion quality
- document deployment/runbook basics

### Outputs
- stable MVP candidate

---

## Suggested implementation order inside each workstream

## Backend first priorities
1. data model and migrations
2. create/list/detail APIs
3. patch API
4. artifact APIs
5. portfolio summary API
6. integration adapters

## Frontend first priorities
1. design tokens and layout primitives
2. list page
3. create page
4. detail page
5. artifact UI
6. portfolio page
7. polish and interaction refinement

## Integration first priorities
1. Salesforce account/opportunity read context
2. BVA summary read context
3. external links
4. scheduled sync/update path

---

## Environment strategy

## Development environment
Should support:
- local UI
- local API
- local database
- mock integration mode for Salesforce/BVA if needed

## Test environment
Should support:
- realistic seeded engagements
- simulated external data responses
- portfolio summary verification

## Production MVP environment
Should support:
- scheduled refresh jobs
- integration credentials/secrets
- basic monitoring and logging

---

## Suggested milestones

## Milestone 1: Local core workflow works
Definition:
- create engagement
- list engagement
- edit engagement
- add/remove artifact reference

## Milestone 2: UI quality baseline is visible
Definition:
- design tokens implemented
- list/detail pages feel premium and modern
- empty/loading/error states are intentional and polished

## Milestone 3: Detail page shows external context
Definition:
- Salesforce account/opportunity context visible
- BVA summary visible
- source-system links working

## Milestone 4: Portfolio page works
Definition:
- summary metrics visible
- needs-attention logic functional

## Milestone 5: MVP ready for pilot
Definition:
- end-to-end workflow stable
- integration assumptions validated
- docs and basic runbook ready
- visual and interaction quality meets product bar

---

## Testing strategy

## Backend tests
- create engagement validation
- patch engagement validation
- artifact create/delete behavior
- normalized detail composition
- portfolio summary computation

## Frontend tests
- create form submission
- section edit/save behavior
- table rendering/filter behavior
- artifact UI behavior
- error and loading state rendering

## Design quality review
Review against:
- typography hierarchy
- spacing consistency
- status badge consistency
- table density and readability
- card and layout polish
- transition smoothness
- empty/loading/error state quality

## Integration tests
- Salesforce read adapter behavior
- BVA summary adapter behavior
- missing external data handling
- scheduled sync/update logic

## Manual UAT scenarios
- opportunity-led engagement creation
- account-led engagement creation
- BVA-linked engagement review
- executive prep update
- artifact link add/remove
- portfolio review inspection

---

## Risks and mitigations

## Risk 1: overbuilding before real usage
Mitigation:
- stay within MVP field and page scope
- avoid advanced automation early

## Risk 2: source-of-truth confusion
Mitigation:
- keep read-only and editable fields visually distinct
- enforce write boundaries in API

## Risk 3: brittle integration timing
Mitigation:
- support mock mode and fallback displays
- use summary sync, not deep writeback

## Risk 4: artifact inconsistency
Mitigation:
- require normalized artifact references in cockpit
- do not treat raw pasted links as unmanaged state

## Risk 5: portfolio logic grows too early
Mitigation:
- keep MVP portfolio summary simple and derived

## Risk 6: premium UI gets deferred into “later polish”
Mitigation:
- implement design tokens in Phase 0/2
- make visual quality a milestone gate
- review UX polish continuously, not only at the end

---

## Team task breakdown recommendation

## Backend engineer
Own:
- schema
- API implementation
- validation
- read-model composition
- artifact persistence

## Frontend engineer
Own:
- routes
- pages
- forms
- reusable UI components
- UX states
- design token implementation
- interaction polish

## Integration engineer or shared responsibility
Own:
- Salesforce adapter
- BVA adapter
- scheduled sync/update jobs
- external reference hydration

## Product/architecture owner
Own:
- acceptance of UI boundaries
- validation of source-of-truth decisions
- review of real workflow usefulness
- acceptance of design quality bar

---

## Definition of done for MVP
The MVP is done when:
- users can create and update engagements in the Cockpit UI
- CRM and BVA context can be viewed from the engagement page
- artifact references can be managed
- portfolio summary is visible
- source-of-truth rules are preserved in implementation
- pilot users can operate the workflow without major ambiguity
- the product feels premium, modern, and intentionally designed

## Summary
The implementation plan should deliver the Cockpit in controlled phases:
- first core cockpit records
- then usable UI
- then cross-system context
- then reporting
- then stabilization

That sequence minimizes risk while validating the operating model quickly.
