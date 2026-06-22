# First Prisma Schema Draft

## Purpose
This document defines the first practical Prisma schema draft for the Cockpit prototype.

It is optimized for:
- SQLite during the prototype phase
- simple local development
- low complexity
- future migration to PostgreSQL if broader rollout is approved

## Design principles
The first schema should:
- support the core engagement workflow
- support artifact link management
- support external system references
- avoid premature complexity
- stay readable and easy to evolve

## MVP entities
Start with three core models:
- `Engagement`
- `EngagementExternalRef`
- `EngagementArtifact`

This is enough for the prototype workflow.

## Example starter schema

```prisma name=prisma/schema.prisma
generator client {
  provider = "prisma-client-js"
}

datasource db {
  provider = "sqlite"
  url      = env("DATABASE_URL")
}

enum EngagementType {
  ACCOUNT_PLAN
  DEAL_STRATEGY
  EXEC_BRIEFING
  BVA_SUPPORT
  OTHER
}

enum EngagementStatus {
  NOT_STARTED
  IN_PROGRESS
  BLOCKED
  COMPLETE
  ON_HOLD
}

enum MeddpiccStatus {
  NOT_STARTED
  IN_PROGRESS
  COMPLETE
  NOT_APPLICABLE
}

enum ExecPrepStatus {
  NOT_STARTED
  IN_PROGRESS
  READY
  COMPLETE
}

enum ExternalSystemType {
  SALESFORCE_ACCOUNT
  SALESFORCE_OPPORTUNITY
  BVA_RECORD
  OTHER
}

enum ArtifactType {
  DOC
  SHEET
  SLIDE
  NOTE
  LINK
  OTHER
}

model Engagement {
  id                String              @id @default(cuid())
  name              String
  engagementType    EngagementType
  owner             String
  status            EngagementStatus    @default(NOT_STARTED)
  strategicObjective String?
  summary           String?
  nextMilestone     String?
  nextAction        String?
  sourceEntryPath   String?

  meddpiccStatus    MeddpiccStatus?     
  execPrepStatus    ExecPrepStatus?
  execEventType     String?
  execEventDate     DateTime?

  bvaStatus         String?
  bvaPhase          String?
  bvaLastUpdatedAt  DateTime?

  accountId         String?
  accountName       String?
  opportunityId     String?
  opportunityName   String?

  createdAt         DateTime            @default(now())
  updatedAt         DateTime            @updatedAt

  externalRefs      EngagementExternalRef[]
  artifacts         EngagementArtifact[]
}

model EngagementExternalRef {
  id            String             @id @default(cuid())
  engagementId  String
  systemType    ExternalSystemType
  externalId    String
  label         String?
  url           String?
  createdAt     DateTime           @default(now())

  engagement    Engagement         @relation(fields: [engagementId], references: [id], onDelete: Cascade)

  @@index([engagementId])
  @@index([systemType, externalId])
}

model EngagementArtifact {
  id            String        @id @default(cuid())
  engagementId  String
  artifactType  ArtifactType
  name          String
  path          String
  createdAt     DateTime      @default(now())
  updatedAt     DateTime      @updatedAt

  engagement    Engagement    @relation(fields: [engagementId], references: [id], onDelete: Cascade)

  @@index([engagementId])
}
```

## Why this is a good prototype schema

### 1. Simple enough to implement quickly
The models are intentionally compact.
They cover the main workflow without forcing early normalization.

### 2. Supports your current UI plan
The schema directly supports:
- engagement list
- create engagement
- engagement detail
- editable cockpit-owned fields
- artifact add/remove behavior
- external source references

### 3. Easy to migrate later
When the prototype is funded, you can:
- move SQLite to PostgreSQL
- normalize some optional fields further
- add auth/user tables
- add audit history
- add richer integration records

## Field guidance

## `Engagement`
This is the core working record.

### MVP-owned editable fields
- `name`
- `engagementType`
- `owner`
- `status`
- `strategicObjective`
- `summary`
- `nextMilestone`
- `nextAction`
- `sourceEntryPath`
- `meddpiccStatus`
- `execPrepStatus`
- `execEventType`
- `execEventDate`

### Context convenience fields
These are included for prototype simplicity:
- `accountId`
- `accountName`
- `opportunityId`
- `opportunityName`
- `bvaStatus`
- `bvaPhase`
- `bvaLastUpdatedAt`

For the prototype, it is acceptable to keep some of this denormalized.

## `EngagementExternalRef`
This stores links to source systems.

Examples:
- Salesforce account reference
- Salesforce opportunity reference
- BVA record reference
- other internal references later

This keeps cross-system linkage explicit.

## `EngagementArtifact`
This stores artifact references only.

Important:
- this does **not** store the underlying file itself
- it stores the Cockpit-side reference to the artifact

Examples:
- Google doc link
- presentation link
- spreadsheet link
- internal notes URL

## Recommended first migration workflow
After updating `prisma/schema.prisma`, run:

```bash name=prisma-migrate-dev.sh
npx prisma migrate dev --name init
```

Then generate the client:

```bash name=prisma-generate.sh
npx prisma generate
```

## Recommended first seed data
Create a few example engagements such as:
- one in progress strategic deal
- one exec briefing preparation item
- one BVA support engagement

Each should include:
- realistic status values
- next milestone
- next action
- one or two artifacts
- one or two external refs

This makes the first UI much easier to evaluate.

## Things to avoid in the first schema
Do not add yet:
- auth/user identity tables
- role/permission tables
- audit event tables
- sync job tables
- notification tables
- advanced analytics tables
- extensive lookup/reference normalization

Those can come later if the prototype proves valuable.

## Suggested future extensions
If the project expands later, likely additions are:
- `User`
- `AuditEvent`
- `EngagementNote`
- `SyncRun`
- `Tag`
- richer account/opportunity models

## Summary
This first schema draft is intentionally pragmatic.
It is designed to help you:
- start quickly
- keep the data model understandable
- support the first end-to-end workflow
- preserve a clean migration path for a future funded version
