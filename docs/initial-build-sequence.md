# Initial Build Sequence

## Purpose
This document defines the recommended first build sequence for the Cockpit prototype.

It is optimized for:
- a non-web-specialist builder
- low architectural risk
- fast visible progress
- zero additional cost during the prototype phase

## Core principle
Do not try to build everything at once.

Build in this order:
1. foundation
2. data model
3. backend CRUD
4. frontend shell
5. core user workflow
6. refinement
7. optional future integration

## Phase 1: Create the project foundation

### Goals
- create a runnable app
- choose the stack once
- avoid premature complexity

### Tasks
- initialize Next.js app with TypeScript
- add Primer React
- set up ESLint/formatting
- create `.env.example`
- install Prisma and initialize schema
- configure SQLite locally

### Done when
- app runs locally
- database connection works
- no external paid services are required

## Phase 2: Establish UI foundation early

### Goals
- keep the UI accessible and GitHub-native
- avoid turning the prototype into a generic CRUD page set

### Tasks
- define layout patterns
- define page container primitives
- define button, input, card, badge, and table baseline usage
- define loading and empty state baseline
- define focus and error-state behavior

### Done when
- one sample page already feels clear and GitHub-aligned
- accessibility basics are visible from the start

## Phase 3: Implement the database model

### Goals
- create the prototype persistence layer

### Tasks
- implement Prisma schema for:
  - engagements
  - engagementExternalRefs
  - engagementArtifacts
- create first migration
- verify tables locally
- seed a few realistic records

### Done when
- local DB contains usable sample data
- Prisma client can read/write core records

## Phase 4: Implement the core backend API

### Goals
- make the prototype workflow real at the data/API level

### Tasks
- implement `GET /api/v1/engagements`
- implement `GET /api/v1/engagements/{engagementId}`
- implement `POST /api/v1/engagements`
- implement `PATCH /api/v1/engagements/{engagementId}`
- implement artifact create/delete endpoints
- implement validation and error responses

### Done when
- API can create, list, read, and update engagements
- artifact linking works

## Phase 5: Build the frontend shell

### Goals
- create a navigable app structure

### Tasks
- implement app layout
- implement top navigation
- implement page container patterns
- implement reusable status badge and card primitives
- implement loading, empty, and error states

### Done when
- app feels like an internal product prototype, not just a code demo
- navigation works across placeholder pages

## Phase 6: Build the first real workflow

### Goals
- support real user value as early as possible

### Tasks
- build engagement list page
- build create engagement page
- build engagement detail page
- build artifact add/remove UI
- wire all pages to the real API

### Done when
- user can create an engagement
- user can view it in the list
- user can open the detail page
- user can edit key fields
- user can add/remove artifacts

## Phase 7: Add lightweight portfolio summary

### Goals
- provide simple review visibility for yourself

### Tasks
- implement portfolio summary backend logic
- implement portfolio summary API
- build portfolio page

### Done when
- portfolio page provides a useful summary of active work

## Phase 8: Stabilize and refine

### Goals
- make the prototype reliable and pleasant to use repeatedly

### Tasks
- clean up edge cases
- improve validation messages
- improve loading behavior
- refine readability and layout
- verify keyboard navigation and focus handling
- test seeded scenarios thoroughly

### Done when
- the prototype feels coherent, useful, and accessible

## Optional later phases
Only after the prototype proves useful:
- add Salesforce integration
- add BVA integration
- migrate SQLite to PostgreSQL
- add authentication
- add shared deployment

## Recommended first technical milestones

### Milestone 1
App runs locally.

### Milestone 2
Prisma schema and migrations work with SQLite.

### Milestone 3
Engagement CRUD API works.

### Milestone 4
Engagement list/detail/create pages work.

### Milestone 5
Artifact reference management works.

### Milestone 6
Portfolio page works.

### Milestone 7
Prototype-quality accessibility and polish achieved.

## Important discipline rules

### Rule 1
Do not start with auth, roles, and advanced permissions.

### Rule 2
Do not start with external integrations.

### Rule 3
Do not add paid services during the prototype phase.

### Rule 4
Do not over-split the architecture early.

### Rule 5
Use realistic seeded data early because usability and accessibility are hard to judge on empty screens.

## Summary
The safest and most suitable build sequence for this prototype is:
- establish the stack
- establish the UI foundation
- build the schema
- build the API
- build the engagement workflow
- add portfolio summary
- refine accessibility and usefulness
