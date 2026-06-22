# Design Tokens Recommendation

## Purpose
This document defines a practical starting point for the Cockpit design token system.

It is intended to help implement the premium Apple-like design direction in a repeatable way.

## Goal
Create a UI that feels:
- modern
- calm
- premium
- precise
- consistent

## Token categories
The initial token system should include:
- color
- typography
- spacing
- radius
- shadow
- motion
- layout widths

## Color direction
Use a neutral-first palette.

### Recommended palette structure
- background
- foreground
- muted
- card
- border
- primary
- success
- warning
- danger

### Guidance
- keep saturation restrained
- rely heavily on neutrals
- use accent colors sparingly
- keep status colors refined, not loud

## Typography direction
Use a crisp modern sans-serif stack.

### Recommended token groups
- display
- h1
- h2
- h3
- body
- body-small
- label
- caption

### Guidance
- use stronger hierarchy through size and weight, not excessive color
- prioritize readability
- avoid tiny text as a default

## Spacing direction
Use a clean spacing scale.

### Recommended scale
- 4
- 8
- 12
- 16
- 20
- 24
- 32
- 40
- 48
- 64

### Guidance
- prefer generous spacing between sections
- avoid cramped card internals
- use consistent rhythm across pages

## Radius direction
Use softer modern radii.

### Suggested levels
- small
- medium
- large
- extra-large

### Guidance
- avoid sharp enterprise boxes
- keep radius consistent across cards, inputs, and buttons

## Shadow direction
Use subtle elevation.

### Suggested levels
- none
- low
- medium
- high

### Guidance
- keep shadows soft
- use them to suggest depth, not decoration

## Motion direction
Use restrained motion tokens.

### Suggested timing levels
- fast
- normal
- slow

### Suggested easing
- standard ease-out for hover/open
- smooth ease-in-out for transitions

### Guidance
- motion should support polish, not become noticeable for its own sake

## Layout width direction
Define consistent content widths.

### Suggested widths
- narrow
- standard
- wide
- full

### Guidance
- use a standard content width for most pages
- use wide layouts for data-heavy list views

## MVP implementation advice
Implement tokens in the simplest maintainable way available in your stack.

For example:
- CSS variables for semantic colors
- Tailwind theme extension for spacing, radius, shadows, and typography
- reusable layout/container classes

## Summary
A small, disciplined token system will help you achieve:
- premium consistency
- faster UI decisions
- less visual drift
- easier future refinement
