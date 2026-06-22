# Initial Build Sequence

## Purpose
This document defines the recommended first build sequence for the Cockpit application.

It is optimized for:
- a non-web-specialist builder
- low architectural risk
- fast visible progress
- preserving premium UI quality from the start

## Core principle
Do not try to build everything at once.

Build in this order:
1. foundation
2. data model
3. backend CRUD
4. frontend shell
5. core user workflow
6. integrations
7. reporting
8. polish

## Phase 1: Create the project foundation

### Goals
- create a runnable app
- choose the stack once
- avoid premature complexity

### Tasks
- initialize Next.js app with TypeScript
- add Tailwind CSS
- add shadcn/ui
- set up ESLint/formatting
- create `.env.example`
- connect the repo to Vercel
- create PostgreSQL database
- install Prisma and initialize schema

### Done when
- app runs locally
- app deploys successfully
- database connection works

## Phase 2: Establish visual foundation early

### Goals
- prevent the UI from becoming a generic CRUD dashboard
- create a premium visual baseline from day one

### Tasks
- define typography scale
- define spacing scale
- define color tokens
- define radius and shadow tokens
- define basic page container/layout primitives
- define button, input, card, badge, table baseline styles
- define loading and empty state baseline

### Done when
- one sample page already feels premium
- styling decisions are reusable, not ad hoc

## Phase 3: Implement the database model

### Goals
- create the MVP persistence layer

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
- make the MVP workflow real at the data/API level

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
- app feels like a product, not just a code demo
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

## Phase 7: Add external context

### Goals
- enrich the cockpit without breaking ownership boundaries

### Tasks
- add Salesforce account context read logic
- add Salesforce opportunity context read logic
- add BVA summary read logic
- populate external refs in engagement detail response
- show external links in UI

### Done when
- detail page shows real or mock external context
- source-of-truth boundaries remain clear

## Phase 8: Add portfolio summary

### Goals
- provide lightweight leadership visibility

### Tasks
- implement portfolio summary backend logic
- implement portfolio summary API
- build portfolio page
- style summary metrics and needs-attention list

### Done when
- portfolio page is useful and visually aligned with the rest of the app

## Phase 9: Stabilize and refine

### Goals
- make the app reliable and pleasant to use repeatedly

### Tasks
- clean up edge cases
- improve validation messages
- improve loading behavior
- refine visual hierarchy
- smooth interactions and spacing
- verify responsiveness
- test seeded scenarios thoroughly

### Done when
- the app feels coherent, reliable, and premium

## Recommended first technical milestones

### Milestone 1
App runs locally and in Vercel.

### Milestone 2
Prisma schema and migrations work.

### Milestone 3
Engagement CRUD API works.

### Milestone 4
Engagement list/detail/create pages work.

### Milestone 5
Artifact reference management works.

### Milestone 6
External context appears.

### Milestone 7
Portfolio page works.

### Milestone 8
Pilot-quality polish achieved.

## Important discipline rules

### Rule 1
Do not start with auth, roles, and advanced permissions unless required immediately.

### Rule 2
Do not build integrations before the local cockpit workflow works.

### Rule 3
Do not delay the visual design system until the end.

### Rule 4
Do not over-split the architecture early.

### Rule 5
Use realistic seeded data early because premium UI quality is hard to judge on empty screens.

## Summary
The safest and most suitable build sequence for you is:
- establish the stack
- establish the visual system
- build the schema
- build the API
- build the engagement workflow
- then add integrations and reporting
- then polish to premium quality
