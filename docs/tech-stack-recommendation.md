# Tech Stack Recommendation

## Recommendation summary
For this Cockpit prototype, the recommended stack is:

- **Framework:** Next.js
- **Language:** TypeScript
- **UI system:** Primer React
- **ORM:** Prisma
- **Database:** SQLite
- **Development mode:** local-first during prototype phase
- **Primary access surface:** Microsoft Teams
- **Authentication:** defer until a broader rollout is approved

This is the best balance of:
- zero additional cost
- implementation speed
- maintainability
- accessibility
- GitHub-native UI fit
- suitability for a developer who is not a web frontend specialist
- alignment with the user’s actual working environment in Teams

## Why this stack is the best fit

### 1. Next.js
Next.js gives you:
- routing
- layouts
- server-side rendering if needed later
- API routes / server actions
- strong ecosystem support

For a solo builder, it reduces the number of architectural decisions.

### 2. TypeScript
TypeScript helps prevent avoidable mistakes across:
- API contracts
- frontend state
- backend logic
- database interaction

Because the app has multiple entities and integration boundaries, types will help a lot.

### 3. Primer React
Primer React is recommended because:
- it aligns with GitHub’s design language
- it supports accessible, familiar internal UI patterns
- it reduces UI design decision overhead
- it is a good fit for an internal GitHub-oriented prototype even when accessed through Teams

This remains the best choice for the product UI layer.

### 4. Prisma
Prisma is recommended because:
- it is approachable
- it makes schema and migrations manageable
- it gives typed database queries
- it reduces day-to-day persistence complexity
- it supports a future migration from SQLite to PostgreSQL if needed

### 5. SQLite
SQLite is recommended for the prototype because:
- it has zero infrastructure cost
- it requires no external database account
- it is easy to set up locally
- it is suitable for a mostly single-user prototype

### 6. Microsoft Teams as the primary access surface
Teams should be treated as the intended primary access point because:
- it is already part of the user’s daily workflow
- it lowers friction for regular usage
- it increases the likelihood of real prototype adoption
- it fits the available Microsoft tool environment at no extra cost

This does **not** mean the first implementation should be deeply Teams-specific.
It means the app should be built as a normal web app with Teams as the intended container and entry point.

## Why not other options

### Browser-only positioning
Avoid framing the prototype as browser-only if the real user workflow is Teams-first.
The browser remains the easiest development environment, but not the best long-term access framing for this use case.

### Managed hosting during prototype
Avoid introducing Vercel or other external platforms during the prototype unless you later need shared access.

### Managed PostgreSQL during prototype
A managed Postgres database is not necessary for a primarily single-user prototype.
It adds setup and possible cost too early.

### Highly custom UI stack
Avoid starting with a highly custom Tailwind/shadcn-based design system for this use case.
The internal prototype should prioritize familiarity and accessibility over custom branding.

## Recommended architecture

## App responsibilities
Use one Next.js app for:
- pages/routes
- server-side API handlers
- UI rendering
- MVP orchestration

## Data responsibilities
Use SQLite + Prisma for:
- engagement persistence
- artifact persistence
- external reference persistence

## Integration responsibilities
Use server-side service modules for:
- future Salesforce read adapters
- future BVA read adapters
- future sync/update jobs if needed

## Access responsibilities
Use the app in two modes:
- direct browser access during development
- intended Microsoft Teams access as the primary user-facing shell

## Decision guidance
If you want one simple answer:

**Choose Next.js + TypeScript + Primer React + Prisma + SQLite, and plan for Microsoft Teams as the primary access surface.**

## Future migration path
If the prototype gets management support and funding, the likely next migration would be:
- keep Next.js
- keep TypeScript
- keep Primer React
- keep Prisma
- switch SQLite to PostgreSQL
- add hosting suitable for Teams access
- add authentication / Microsoft identity if needed
- register or package the app appropriately for Teams delivery

## Summary
This stack gives you the best combination of:
- no extra cost
- low setup friction
- accessible internal UI
- strong maintainability
- strong fit with the user’s daily Teams workflow
- future upgrade path without wasted prototype work
