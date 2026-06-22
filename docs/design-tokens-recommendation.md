# Design Tokens Recommendation

## Purpose
This document defines a practical starting point for the Cockpit prototype design system.

It is intended to help implement a GitHub-native and accessible UI in a repeatable way.

## Goal
Create a UI that feels:
- clear
- accessible
- calm
- familiar to GitHub users
- consistent

## Primary direction
This prototype should use **Primer React and Primer design foundations first**.

Design tokens and additional styling should support that direction rather than replace it.

## Token categories
The initial token system should include:
- color
- typography
- spacing
- radius
- border and surface treatment
- focus treatment
- layout widths

## Color direction
Use a restrained, accessible palette aligned with GitHub-native usage.

### Guidance
- use semantic colors carefully
- prioritize contrast and readability
- avoid decorative saturation
- ensure status colors remain distinguishable and accessible

## Typography direction
Use readable, familiar typography with clear hierarchy.

### Recommended token groups
- page title
- section title
- body
- body-small
- label
- caption

### Guidance
- prioritize clarity over stylistic distinctiveness
- avoid overly small text
- maintain strong content hierarchy

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

### Guidance
- preserve readable separation between sections
- avoid cramped forms and cards
- keep spacing consistent across pages

## Radius direction
Use moderate radii consistent with Primer-like patterns.

### Guidance
- do not over-round surfaces
- keep radius use restrained and consistent

## Border and surface direction
Use simple structure with low visual noise.

### Guidance
- use borders only where they improve clarity
- prefer clean section grouping
- avoid heavy boxed enterprise styling

## Focus treatment
Focus behavior is a first-class requirement.

### Guidance
- all interactive elements must have visible focus states
- focus styling should be clear and accessible
- keyboard navigation should be easy to track visually

## Layout width direction
Define consistent content widths.

### Suggested widths
- standard
- wide
- full

### Guidance
- use standard widths for forms and detail pages
- use wider layouts for tables and portfolio views

## MVP implementation advice
Implement tokens in the simplest maintainable way available in the chosen stack.

For this prototype:
- use Primer defaults first
- add a thin layer of local token decisions only where needed
- do not build a large custom design token system prematurely

## Summary
A small, disciplined token layer should help achieve:
- GitHub-native consistency
- accessibility
- lower design risk
- easier future refinement if the prototype becomes a funded product
