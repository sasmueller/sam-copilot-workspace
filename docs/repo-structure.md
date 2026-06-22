# Repository Structure Recommendation

## Purpose
This document defines a practical repository structure for the Cockpit web application.

The structure is optimized for:
- a Next.js full-stack app
- clear feature boundaries
- manageable complexity for a non-frontend specialist
- future growth without early overengineering

## Recommended repository model
Use a **single repository** with a **single Next.js application** for MVP.

Do not start with microservices or a multi-repo architecture.

## Top-level structure
```text name=repo-structure.txt
.
├── docs/
├── prisma/
├── public/
├── src/
│   ├── app/
│   ├── components/
│   ├── features/
│   ├── lib/
│   ├── server/
│   ├── styles/
│   └── types/
├── .env.example
├── package.json
├── tsconfig.json
└── README.md
```

## Directory guidance

## `docs/`
Stores product, architecture, API, schema, and implementation documents.

## `prisma/`
Stores Prisma schema and migrations.

Recommended contents:
```text name=prisma-structure.txt
prisma/
├── schema.prisma
└── migrations/
```

## `public/`
Stores static assets.

Examples:
- logos
- icons
- illustrations
- placeholder images

## `src/`
Application source code.

### `src/app/`
Next.js App Router structure.

Recommended route layout:
```text name=app-structure.txt
src/app/
├── layout.tsx
├── page.tsx
├── globals.css
├── engagements/
│   ├── page.tsx
│   ├── new/
│   │   └── page.tsx
│   └── [engagementId]/
│       └── page.tsx
└── portfolio/
    └── page.tsx
```

### `src/components/`
Reusable shared UI components.

Examples:
- buttons
- inputs
- cards
- badges
- dialogs
- table primitives
- loading/error states

Suggested structure:
```text name=components-structure.txt
src/components/
├── ui/
├── layout/
└── feedback/
```

### `src/features/`
Feature-based UI and logic grouping.

Recommended feature folders:
```text name=features-structure.txt
src/features/
├── engagements/
├── artifacts/
├── portfolio/
└── shared/
```

Use this folder for:
- feature-specific components
- forms
- view models
- feature hooks if needed
- feature utilities

### `src/lib/`
General-purpose utilities and helpers.

Examples:
- formatting helpers
- date utilities
- badge color mapping
- class name helpers
- config constants

### `src/server/`
Server-side app logic.

Recommended structure:
```text name=server-structure.txt
src/server/
├── db/
├── repositories/
├── services/
├── integrations/
├── api/
└── validators/
```

Use this for:
- Prisma client setup
- database access functions
- orchestration services
- Salesforce/BVA adapters
- request validation
- API response composition

### `src/styles/`
Optional additional styling structure.

Use for:
- design tokens
- motion definitions
- theme utilities if needed

### `src/types/`
Shared TypeScript types.

Use for:
- API response types
- domain models
- form types
- integration DTOs

## Recommended detailed organization

## UI layer principles
- keep route files small
- move page-specific UI into feature folders
- keep shared components generic
- keep styling decisions centralized where possible

## Server layer principles
- keep route handlers thin
- put business logic into services
- put DB access into repositories or Prisma-layer modules
- isolate integration code from core business logic

## Suggested API organization
If using route handlers, use:
```text name=api-route-structure.txt
src/app/api/
├── v1/
│   ├── engagements/
│   │   ├── route.ts
│   │   └── [engagementId]/
│   │       ├── route.ts
│   │       └── artifacts/
│   │           ├── route.ts
│   │           └── [artifactId]/
│   │               └── route.ts
│   └── portfolio/
│       └── summary/
│           └── route.ts
```

## Suggested feature composition

### `src/features/engagements/`
Could contain:
```text name=engagement-feature-structure.txt
src/features/engagements/
├── components/
├── forms/
├── actions/
├── queries/
├── mappers/
└── types.ts
```

### `src/features/artifacts/`
Could contain:
```text name=artifact-feature-structure.txt
src/features/artifacts/
├── components/
├── forms/
└── types.ts
```

### `src/features/portfolio/`
Could contain:
```text name=portfolio-feature-structure.txt
src/features/portfolio/
├── components/
├── queries/
└── types.ts
```

## Keep it simple rule
If a folder is empty or artificial, do not create it yet.
The structure should guide growth, not create ceremony.

## MVP implementation priority structure
At the beginning, you mainly need:
- `src/app/`
- `src/components/ui/`
- `src/features/engagements/`
- `src/server/db/`
- `src/server/services/`
- `src/server/integrations/`
- `prisma/`

## Summary
The best repository structure for you is:
- one repo
- one Next.js app
- Prisma for schema/migrations
- route-driven app layer
- reusable components layer
- feature-based UI grouping
- thin route handlers with server services behind them
