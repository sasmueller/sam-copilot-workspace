# Cockpit Frontend Spec

## Purpose
This document defines the MVP frontend implementation spec for the Sales Cockpit prototype.

It translates the UI blueprint into concrete frontend requirements:
- routes
- pages
- components
- state behavior
- data-fetching needs
- edit behavior
- accessibility expectations
- GitHub-native UI expectations
- Microsoft Teams container expectations

## Frontend goals
The frontend must:
- provide a useful engagement workspace
- allow low-friction editing of cockpit-owned fields
- make source-of-truth boundaries visually clear
- present cross-system context in one place
- support artifact link management
- provide a simple portfolio view
- deliver an accessible and GitHub-native product feel
- work well inside Microsoft Teams as the primary access surface

## Visual and interaction requirement
The Cockpit UI should feel like a **GitHub-native, accessible, calm, and polished internal tool**.

This means the frontend implementation should emphasize:
- clarity over novelty
- strong readability
- familiar GitHub-aligned interaction patterns
- accessible form and table behavior
- restrained visual styling
- polished loading/empty/error states

This prototype should not prioritize a highly custom branded visual system.
It should prioritize accessibility, familiarity, and speed of implementation.

## Runtime and access model
The frontend should be built as a standard web application, but with this usage model:
- local browser access during development
- Microsoft Teams as the intended primary user entry point
- likely future delivery inside Teams as a tab or embedded web experience

### Important implication
Do not build the first version as a deeply Teams-specific application.
Build a normal Next.js app, but ensure the layout and interaction model work well inside a Teams container.

## Recommended frontend architecture
For the prototype, use:
- component-based Next.js app
- route-driven page model
- API-backed state
- minimal client-side business logic
- server-owned canonical validation
- Primer-based UI components where possible

### Recommendation
Keep the frontend thin:
- fetch normalized view models from the API
- avoid reproducing sync logic in the browser
- avoid client-side orchestration rules beyond presentation and local form behavior

---

## 1. Routes

### Required routes
- `/engagements`
- `/engagements/new`
- `/engagements/:engagementId`
- `/portfolio`

---

## 2. Design system guidance

## Primary UI system
Use Primer React as the primary UI system.

### Why
- GitHub-native visual language
- better internal familiarity for GitHub users
- accessibility-oriented patterns
- lower design overhead for a prototype
- still appropriate when the app is surfaced through Teams

## Styling guidance
- prefer Primer primitives and conventions first
- add only limited custom styling where needed
- keep the UI clean, calm, and readable
- avoid a visually dense admin-dashboard feel

---

## 3. Pages

## Engagement list page
Route:
- `/engagements`

### Responsibilities
- fetch engagement list
- display filters
- display engagement table
- link to detail page
- link to create page

### Required filters
- owner
- engagement type
- status
- BVA status
- exec prep status
- text search

### Required empty state
If no engagements exist:
- show a clear empty state
- include CTA to create engagement
- ensure the empty state feels intentional and readable

## Create engagement page
Route:
- `/engagements/new`

### Responsibilities
- display create form
- validate required fields
- submit to create API
- redirect to engagement detail after success

## Engagement detail page
Route:
- `/engagements/:engagementId`

### Responsibilities
- fetch normalized engagement detail
- render sectioned workspace
- allow editing of cockpit-owned fields
- allow artifact reference creation/removal
- show read-only external context
- expose source-system links

## Portfolio page
Route:
- `/portfolio`

### Responsibilities
- fetch portfolio summary
- display summary metrics
- display “needs attention” list

---

## 4. Data-fetching contract

## Engagement list page
API:
- `GET /api/v1/engagements`

## Create engagement page
API:
- `POST /api/v1/engagements`

## Engagement detail page
API:
- `GET /api/v1/engagements/{engagementId}`
- `PATCH /api/v1/engagements/{engagementId}`
- `POST /api/v1/engagements/{engagementId}/artifacts`
- `DELETE /api/v1/engagements/{engagementId}/artifacts/{artifactId}`

## Portfolio page
API:
- `GET /api/v1/portfolio/summary`

---

## 5. Page composition

## Engagement list page layout
Sections:
- page header
- filter bar
- engagement table
- pagination footer

### Page header
Show:
- page title
- create engagement button

### Filter bar
Controls:
- search input
- owner filter
- type filter
- status filter
- BVA status filter
- exec prep status filter

### Engagement table columns
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
- last updated

### Visual guidance for the table
The table should prioritize:
- readability
- keyboard usability
- clear row focus/hover states
- restrained status indication

## Engagement detail page layout
Sections:
- page header
- strategic overview card
- BVA card
- MEDDPICC card
- executive prep card
- artifact card
- CRM context card

### Header actions
- open Salesforce account
- open Salesforce opportunity if present
- open BVA source if present

### Visual guidance for detail page
The page should feel:
- clear
- structured
- familiar
- accessible
- lightweight but professional

## Portfolio page layout
Sections:
- summary metrics row
- engagement counts by type
- engagement counts by status
- BVA summary block
- upcoming exec interactions block
- needs attention list

---

## 6. Components

## Required reusable components
- `PageHeader`
- `StatusBadge`
- `FilterBar`
- `EngagementTable`
- `FieldRow`
- `EditableTextField`
- `EditableTextArea`
- `EditableSelect`
- `EditableDateField`
- `DetailCard`
- `ArtifactList`
- `ArtifactForm`
- `ExternalLinkButton`
- `EmptyState`
- `LoadingState`
- `ErrorState`
- `MetricCard`

## Component guidance
Prefer implementing these with Primer-compatible patterns and primitives where practical.

---

## 7. Form behavior

## Create engagement form
### Required inputs
- engagement name
- engagement type
- engagement owner
- account ID or account selector
- strategic objective
- next milestone
- next action
- status

### Optional inputs
- opportunity ID or selector
- source entry path
- summary

### Validation behavior
- required fields validated client-side for usability
- server remains source of truth for validation
- show inline field errors where possible
- ensure validation messaging is accessible

## Edit behavior on detail page
Editable fields:
- engagement name
- engagement type
- engagement owner
- strategic objective
- next milestone
- next action
- status
- summary
- source entry path
- MEDDPICC status
- exec prep status
- exec event type
- exec event date

### Recommended MVP choice
Use **explicit Save per section** for simplicity and clarity.

---

## 8. Read-only context behavior

## CRM context
Read-only display:
- account name
- industry
- segment
- region
- opportunity name
- stage
- opportunity owner

### UX guidance
Show a clear indicator that these fields are sourced from Salesforce.

## BVA summary
Read-only display:
- BVA status
- BVA phase
- BVA last updated

### UX guidance
Provide clear link-outs to the BVA source or artifact.

---

## 9. Artifact management behavior

## Display behavior
Each artifact item should show:
- artifact type
- artifact name
- artifact path or domain hint
- open link action

## Create behavior
Artifact form fields:
- artifact type
- artifact name
- artifact path

## Delete behavior
Allow deleting cockpit reference records.
Do not delete the underlying external file.

---

## 10. State management guidance

### Local component state
Use for:
- form inputs
- open/closed panels
- local loading flags
- local validation messages

### Shared/page state
Use for:
- fetched engagement detail
- engagement list
- filters
- portfolio summary

### Recommendation
Use a lightweight data-fetching pattern and avoid overengineering state management for the prototype.

---

## 11. Error handling

## Required error states
- list load failure
- detail load failure
- create failure
- save failure
- artifact add/remove failure

## UX guidance
- show actionable error messages
- preserve unsaved user input where possible
- allow retry actions
- ensure error feedback is accessible and calm

---

## 12. Loading behavior

## Required loading states
- list loading
- detail loading
- portfolio loading
- form submission loading
- artifact mutation loading

### UX guidance
Prefer non-blocking section-level loading where practical.
Loading treatments should feel intentional and readable.

---

## 13. Teams container guidance

### Minimum Teams-aware expectations
The frontend should:
- work inside narrower container widths than a full desktop browser
- avoid relying on large full-width layouts only
- keep important actions visible without excessive scrolling
- use responsive spacing and card stacking where needed
- keep headings and tables readable in embedded contexts

### Practical layout guidance
- prefer section cards that can stack vertically
- avoid over-wide multi-column layouts for essential workflows
- keep the engagement detail page readable in a constrained width
- ensure forms remain usable inside an embedded tab view

### Navigation guidance
The app should still function as a normal web app, but navigation patterns should remain simple enough to feel comfortable inside Teams.

---

## 14. Accessibility and usability guidance

### Minimum requirements
- keyboard navigable forms
- visible labels
- accessible button names
- semantic table or list markup
- adequate badge contrast
- error text associated with fields
- visible focus states

### Usability guidance
- keep editing fast
- keep layout readable
- highlight next action and next milestone prominently
- make external navigation obvious

---

## 15. Prototype frontend non-goals
- complex offline behavior
- advanced role-based personalization
- deep audit-history UI
- drag-and-drop workflow boards
- embedded Power BI authoring
- embedded BVA authoring
- advanced analytics controls
- highly custom visual branding system
- deep Teams-specific custom extension behavior in the first phase

## Summary
The prototype frontend should be a thin but effective operating workspace:
- clear routes
- simple forms
- normalized API consumption
- section-based engagement detail
- artifact link management
- lightweight portfolio visibility
- GitHub-native accessible UI via Primer
- practical usability inside Microsoft Teams
