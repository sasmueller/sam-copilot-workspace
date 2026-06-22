# Cockpit Design System Direction

## Purpose
This document defines the intended visual and interaction direction for the Sales Cockpit web application.

It ensures the Cockpit is implemented as a premium modern product experience rather than a generic enterprise admin tool.

This document complements:
- `docs/cockpit-ui-blueprint.md`
- `docs/cockpit-frontend-spec.md`
- `docs/mvp-decisions.md`

## Design ambition
The Cockpit should feel like a **state-of-the-art web app with Apple-like clarity, restraint, polish, and confidence**.

This means:
- premium but not flashy
- minimal but not empty
- elegant but still practical
- calm but still information-rich
- highly intentional in spacing, type, and motion

This does **not** mean copying Apple branding, icons, or layout patterns literally.
It means using a similarly high standard of design craft.

## Core design principles

### 1. Clarity first
Every screen should make the user feel oriented immediately.

### 2. Restraint over decoration
Use fewer visual devices, but execute them well.

### 3. Content is the hero
The interface should support the workflow and the information, not compete with it.

### 4. Premium motion
Animation should be subtle, smooth, and purposeful.

### 5. Calm confidence
The product should feel modern and sophisticated, not loud or over-styled.

## Visual language

## Typography
The type system should be one of the strongest parts of the product.

Guidance:
- crisp, modern sans-serif typography
- strong hierarchy between page titles, section titles, labels, and body text
- readable body copy with generous line height
- restrained use of small/meta text

## Spacing
Spacing should feel generous and intentional.

Guidance:
- use a consistent spacing scale
- prefer whitespace over dividers when possible
- create strong visual rhythm between sections
- avoid cramped forms and cards

## Color
Color should be restrained.

Guidance:
- mostly neutral base palette
- color used intentionally for status, emphasis, and actions
- avoid rainbow dashboards or saturated surfaces
- preserve calmness in the overall composition

## Elevation and surfaces
Use subtle depth rather than heavy boxes.

Guidance:
- soft shadows
- soft radii
- low-noise surfaces
- minimal borders
- clear but understated separation between sections

## Motion
Motion should communicate quality and responsiveness.

Guidance:
- subtle transitions
- no unnecessary flourish
- use motion for hover, focus, section reveal, save confirmation, and page transitions
- keep timing short and responsive

## Interaction design guidance

## Buttons
Buttons should feel modern and confident.

Guidance:
- clear primary action
- restrained secondary actions
- avoid too many competing button styles

## Forms
Forms should feel lightweight and approachable.

Guidance:
- generous field spacing
- obvious labels
- clean validation states
- avoid dense enterprise-form aesthetics

## Tables and lists
Tables should feel premium, not spreadsheet-like.

Guidance:
- lightweight dividers if any
- strong row spacing
- subtle hover/focus treatment
- typography-led readability

## Cards and panels
Cards should structure the page without overwhelming it.

Guidance:
- use cards to group meaningfully
- avoid over-nesting
- keep card surfaces simple and elegant

## Empty, loading, and error states
These states should feel intentionally designed.

Guidance:
- calm, polished placeholders
- clear copy
- subtle loading treatments
- errors that are helpful without feeling alarming

## What to avoid
Avoid:
- generic admin template styling
- overly dense dashboard layouts
- too many borders and boxes
- visual clutter
- excessive iconography
- overly saturated status treatments
- abrupt or jarring transitions
- “old enterprise software” feel

## MVP-specific application
For MVP, premium quality should show up most strongly in:
- page layout
- typography
- spacing
- card design
- status badges
- loading and empty states
- transition polish

Do not try to solve premium quality with branding alone.
The quality should come from the system itself.

## Implementation guidance
The frontend should implement this direction via:
- design tokens
- reusable layout and component primitives
- consistent spacing and typography scales
- consistent motion timing
- a small number of well-defined component variants

## Success test
The UI should feel:
- modern
- calm
- premium
- easy to read
- fast to understand
- pleasant to use repeatedly

If it feels like a generic CRUD dashboard, the implementation has missed the target.

## Summary
The Cockpit should be built as a premium state-of-the-art web app.

Its design direction should emphasize:
- clarity
- restraint
- hierarchy
- whitespace
- subtle depth
- polished motion
- enterprise usefulness without enterprise heaviness
