# Cockpit Frontend Spec

## Purpose
This document defines the MVP frontend implementation spec for the Sales Cockpit web application.

It translates the UI blueprint into concrete frontend requirements:
- routes
- pages
- components
- state behavior
- data-fetching needs
- edit behavior
- visual system expectations

## Frontend goals
The frontend must:
- provide a useful engagement workspace
- allow low-friction editing of cockpit-owned fields
- make source-of-truth boundaries visually clear
- present cross-system context in one place
- support artifact link management
- provide a simple portfolio view
- deliver a premium state-of-the-art product feel

## Visual and interaction requirement
The Cockpit UI should feel like a **modern premium web product with Apple-like clarity and polish**.

This means the frontend implementation should emphasize:
- elegant typography
- generous spacing
- restrained color use
- subtle elevation and depth
- calm layouts
- smooth motion and transitions
- high-quality empty/loading/error states
- premium responsiveness across screen sizes

This does not mean imitating Apple branding directly.
It means implementing a high-craft product experience.

## Recommended frontend architecture
For MVP, use:
- component-based SPA or hybrid app
- route-driven page model
- API-backed state
- minimal client-side business logic
- server-owned canonical validation
- token-driven styling system

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

### Optional future routes
- `/settings`
- `/reports`
- `/artifacts/:artifactId`

---

## 2. Design token guidance

## Required token categories
The frontend should define a design-token layer for:
- color
- typography
- spacing
- radius
- shadow/elevation
- motion
- layout widths

## Typography guidance
Typography should feel modern, crisp, and premium.

Suggested approach:
- clear display size for page headers
- strong section-title hierarchy
- highly readable body text
- restrained label and meta text styling

## Spacing guidance
Use a consistent spacing scale and avoid cramped layouts.

The UI should feel breathable and deliberate.

## Radius and elevation guidance
Use:
- soft modern border radii
- subtle shadows
- minimal borders where possible
- layered surfaces rather than harsh boxes

## Motion guidance
Motion should be:
- subtle
- fast
- intentional
- non-distracting

Recommended motion uses:
- page transitions
- section reveal
- hover states
- save feedback
- table row hover/focus

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
- ensure the empty state feels intentional and polished, not placeholder-like

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

### Data needed
- list items
- pagination metadata
- filter-compatible fields

## Create engagement page
API:
- `POST /api/v1/engagements`

### Supporting data
If selectors need remote data later, add lookup endpoints.
For MVP this can start with simple account/opportunity resolution patterns as available.

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
The table should feel lightweight and premium:
- minimal grid noise
- strong row spacing
- subtle hover treatment
- crisp typography
- status badges that do not overwhelm content

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
The page should feel like a premium workspace:
- strong typographic hierarchy
- generous vertical rhythm
- card grouping with subtle separation
- uncluttered action areas
- editable areas that are obvious but not visually heavy

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

## Suggested table row behavior
Clicking a row opens the engagement detail page.

## Suggested status badge behavior
Use consistent color mapping for:
- engagement status
- BVA status
- MEDDPICC status
- exec prep status

## Component style guidance
Components should be:
- quiet by default
- polished on interaction
- consistent in spacing and radius
- visually unified across pages

Avoid a generic admin-dashboard aesthetic.

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

### Save model recommendation
Use one of:
- explicit Save button per section
- autosave with debounce

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
Use a lightweight query/state library or framework-native data layer.
Avoid overengineering state management for MVP.

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
- error states should still feel designed and calm, not abrupt or raw

---

## 12. Loading behavior

## Required loading states
- list loading
- detail loading
- portfolio loading
- form submission loading
- artifact mutation loading

### UX guidance
Prefer skeletons or lightweight section loaders over blocking the whole page when possible.
Loading treatments should feel refined and intentional.

---

## 13. Accessibility and usability guidance

### Minimum requirements
- keyboard navigable forms
- visible labels
- accessible button names
- semantic table or list markup
- adequate badge contrast
- error text associated with fields

### Usability guidance
- keep editing fast
- keep layout readable
- highlight next action and next milestone prominently
- make external navigation obvious

---

## 14. MVP frontend non-goals
- complex offline behavior
- advanced role-based personalization
- deep audit-history UI
- drag-and-drop workflow boards
- embedded Power BI authoring
- embedded BVA authoring
- advanced analytics controls

## Summary
The MVP frontend should be a thin but effective operating workspace:
- clear routes
- simple forms
- normalized API consumption
- section-based engagement detail
- artifact link management
- lightweight portfolio visibility
- premium, state-of-the-art visual and interaction quality
