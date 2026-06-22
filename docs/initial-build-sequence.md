# Initial Build Sequence

## Goal
This document recommends the most practical order for building the Cockpit prototype.

The goal is to:
- reduce startup complexity
- get to a usable prototype quickly
- validate the core workflow before layering on integrations
- keep development local-first
- keep the product suitable for Teams-first usage in practice

## Recommended build philosophy
Build the prototype in this order:
1. make the app run locally
2. make the core data model work
3. make one end-to-end workflow work
4. make the UI readable and useful
5. validate that the app layout will work well in Microsoft Teams
6. add more breadth only after the core flow is credible

Do not start by optimizing deployment, external integrations, or complex architecture.

---

## Phase 1: App foundation

### Objective
Create a running local application with the minimum structure required to build on safely.

### Deliverables
- Next.js app initialized
- TypeScript configured
- Primer React installed
- basic application layout created
- repository structure established

### Why this first
This gives you a working shell quickly and avoids overthinking architecture before anything renders.

### Notes
At this stage, treat the app as a normal browser-based web app during development.
Do not block progress on Teams-specific packaging or hosting.

---

## Phase 2: Local persistence foundation

### Objective
Create the simplest useful data layer.

### Deliverables
- Prisma installed
- SQLite configured
- first Prisma schema created
- first migration applied
- Prisma client generated

### Why this second
The product revolves around engagement records and related metadata.
You need a stable persistence layer early.

---

## Phase 3: Seed realistic sample data

### Objective
Avoid building UI against empty screens.

### Deliverables
- seed script added
- several realistic engagement examples created
- artifacts and external references seeded

### Why this third
The product is much easier to design and validate when it already has believable content.

---

## Phase 4: Core engagement list workflow

### Objective
Get the first useful screen working.

### Deliverables
- `/engagements` page
- simple engagement table
- basic status display
- basic filtering skeleton
- API route for list retrieval

### Why this fourth
The list page becomes your first proof that the prototype is becoming operational.

---

## Phase 5: Create engagement workflow

### Objective
Make the app capable of producing its own records.

### Deliverables
- `/engagements/new` page
- basic create form
- create API handler
- redirect to detail page after creation

### Why this fifth
Once you can create new records, the prototype starts to feel real.

---

## Phase 6: Engagement detail workflow

### Objective
Create the main operating workspace.

### Deliverables
- `/engagements/[engagementId]` page
- section-based detail layout
- editable cockpit-owned fields
- save behavior per section
- read-only CRM/BVA context placeholders

### Why this sixth
This is where most of the product value lives.

---

## Phase 7: Artifact management workflow

### Objective
Support linked working materials.

### Deliverables
- artifact list display
- artifact add form
- artifact removal action

### Why this seventh
Artifact linking is useful, but not as foundational as basic engagement CRUD.

---

## Phase 8: Teams suitability validation

### Objective
Confirm that the app works well for the intended primary access surface.

### Deliverables
- review page layouts at narrower widths
- confirm important actions remain visible
- confirm tables remain readable enough
- confirm cards/forms stack appropriately
- identify any layout issues that would make Teams usage uncomfortable

### Why this eighth
Teams is the intended daily access surface, but it should influence the app through layout validation rather than by driving premature platform complexity.

### Important note
At this stage, the goal is not deep Teams integration.
The goal is to ensure the app is comfortable to use in a Teams-hosted context.

---

## Phase 9: Portfolio view

### Objective
Add lightweight roll-up visibility.

### Deliverables
- `/portfolio` page
- summary metrics
- needs attention section
- counts by type/status

### Why this ninth
Portfolio visibility is valuable, but it depends on the core engagement workflow being meaningful first.

---

## Phase 10: External integration refinement

### Objective
Improve realism after the prototype already works.

### Possible deliverables
- cleaner Salesforce link handling
- BVA reference enrichment
- import/sync helpers
- stronger view-model shaping for the UI

### Why this later
External integrations are important, but they should not block the first usable prototype.

---

## What not to build first
Do **not** start with:
- authentication
- deployment automation
- hosted infrastructure
- PostgreSQL
- Power BI embedding
- Salesforce sync jobs
- advanced reporting
- advanced role models
- deeply custom Teams extensions

These can all come later if the prototype proves valuable.

---

## Recommended implementation sequence summary
If you want the short version, build in this order:

1. Next.js app shell
2. Primer setup
3. Prisma + SQLite
4. schema + migration
5. seed data
6. engagement list page
7. create page
8. detail page
9. artifact management
10. Teams layout suitability check
11. portfolio page
12. later integration refinement

---

## Success criteria for MVP progress
You are on the right track when:
- the app runs locally without friction
- seeded data makes the UI look realistic
- you can create an engagement
- you can open the engagement detail page
- you can edit cockpit-owned fields
- you can manage artifact references
- the UI feels clean and readable
- the layout appears suitable for eventual Teams-based usage

---

## Summary
The best build order is the one that gets you to a credible working flow quickly.

For this prototype, that means:
- local-first development
- simple persistence
- one end-to-end workflow early
- Teams-aware layout validation
- integrations and platform complexity later
