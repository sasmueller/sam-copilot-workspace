# Initial Prisma Seed Script Draft

## Purpose
This document defines a practical first seed script draft for the Cockpit prototype.

It is designed to:
- populate realistic sample data quickly
- support UI development early
- support local SQLite development
- keep the seed logic understandable

## Why seed data matters
A prototype is easier to evaluate when:
- pages are not empty
- tables have realistic rows
- statuses can be seen in context
- detail pages show complete examples
- artifact and external reference sections are populated

## Recommended seed strategy
For the first seed pass, create:
- 3 engagements
- 1 to 3 artifacts per engagement
- 1 to 3 external references per engagement

The sample records should cover different states and use cases.

## Suggested seeded scenarios

### Scenario 1: Strategic deal in progress
Use this to test:
- active engagement list display
- in-progress statuses
- next milestone and next action visibility

### Scenario 2: Executive briefing preparation
Use this to test:
- executive prep fields
- event date display
- briefing-specific artifact links

### Scenario 3: BVA support engagement
Use this to test:
- BVA summary fields
- linked opportunity context
- mixed artifact types

## Recommended file location
Create the seed script in:

```text name=seed-file-path.txt
prisma/seed.ts
```

## Example seed script draft

```typescript name=prisma/seed.ts
import { PrismaClient, ArtifactType, EngagementStatus, EngagementType, ExecPrepStatus, ExternalSystemType, MeddpiccStatus } from '@prisma/client'

const prisma = new PrismaClient()

async function main() {
  await prisma.engagementArtifact.deleteMany()
  await prisma.engagementExternalRef.deleteMany()
  await prisma.engagement.deleteMany()

  const strategicDeal = await prisma.engagement.create({
    data: {
      name: 'Acme Global Expansion Deal Support',
      engagementType: EngagementType.DEAL_STRATEGY,
      owner: 'Sascha Mueller',
      status: EngagementStatus.IN_PROGRESS,
      strategicObjective: 'Align account strategy, stakeholders, and proposal narrative for Q3 expansion motion.',
      summary: 'Primary working record for strategic pursuit support.',
      nextMilestone: 'Internal pursuit review with regional leadership',
      nextAction: 'Finalize stakeholder map and deal narrative before Thursday review',
      sourceEntryPath: 'Sales Cockpit / Acme / Q3 Expansion',
      meddpiccStatus: MeddpiccStatus.IN_PROGRESS,
      execPrepStatus: ExecPrepStatus.NOT_STARTED,
      accountId: 'SF-ACC-1001',
      accountName: 'Acme Corporation',
      opportunityId: 'SF-OPP-2001',
      opportunityName: 'Acme EMEA Expansion FY26',
      bvaStatus: 'In progress',
      bvaPhase: 'Discovery',
      bvaLastUpdatedAt: new Date('2026-06-18T10:00:00.000Z'),
      artifacts: {
        create: [
          {
            artifactType: ArtifactType.DOC,
            name: 'Deal Strategy Working Doc',
            path: 'https://example.com/docs/acme-deal-strategy'
          },
          {
            artifactType: ArtifactType.SLIDE,
            name: 'Regional Review Deck',
            path: 'https://example.com/slides/acme-regional-review'
          }
        ]
      },
      externalRefs: {
        create: [
          {
            systemType: ExternalSystemType.SALESFORCE_ACCOUNT,
            externalId: 'SF-ACC-1001',
            label: 'Salesforce Account',
            url: 'https://example.com/salesforce/account/SF-ACC-1001'
          },
          {
            systemType: ExternalSystemType.SALESFORCE_OPPORTUNITY,
            externalId: 'SF-OPP-2001',
            label: 'Salesforce Opportunity',
            url: 'https://example.com/salesforce/opportunity/SF-OPP-2001'
          },
          {
            systemType: ExternalSystemType.BVA_RECORD,
            externalId: 'BVA-3001',
            label: 'BVA Workspace',
            url: 'https://example.com/bva/BVA-3001'
          }
        ]
      }
    }
  })

  const execBriefing = await prisma.engagement.create({
    data: {
      name: 'Northstar Executive Briefing Preparation',
      engagementType: EngagementType.EXEC_BRIEFING,
      owner: 'Sascha Mueller',
      status: EngagementStatus.IN_PROGRESS,
      strategicObjective: 'Prepare executive briefing aligned to customer priorities and current opportunity posture.',
      summary: 'Tracks briefing readiness, materials, and executive actions.',
      nextMilestone: 'Executive prep review',
      nextAction: 'Confirm attendee list and finalize briefing narrative',
      sourceEntryPath: 'Sales Cockpit / Northstar / Exec Briefing',
      meddpiccStatus: MeddpiccStatus.NOT_APPLICABLE,
      execPrepStatus: ExecPrepStatus.IN_PROGRESS,
      execEventType: 'Customer executive briefing',
      execEventDate: new Date('2026-07-02T15:00:00.000Z'),
      accountId: 'SF-ACC-1002',
      accountName: 'Northstar Health',
      opportunityId: 'SF-OPP-2002',
      opportunityName: 'Northstar Platform Renewal',
      artifacts: {
        create: [
          {
            artifactType: ArtifactType.SLIDE,
            name: 'Executive Briefing Deck',
            path: 'https://example.com/slides/northstar-exec-briefing'
          },
          {
            artifactType: ArtifactType.NOTE,
            name: 'Speaker Notes',
            path: 'https://example.com/notes/northstar-speaker-notes'
          }
        ]
      },
      externalRefs: {
        create: [
          {
            systemType: ExternalSystemType.SALESFORCE_ACCOUNT,
            externalId: 'SF-ACC-1002',
            label: 'Salesforce Account',
            url: 'https://example.com/salesforce/account/SF-ACC-1002'
          },
          {
            systemType: ExternalSystemType.SALESFORCE_OPPORTUNITY,
            externalId: 'SF-OPP-2002',
            label: 'Salesforce Opportunity',
            url: 'https://example.com/salesforce/opportunity/SF-OPP-2002'
          }
        ]
      }
    }
  })

  const bvaSupport = await prisma.engagement.create({
    data: {
      name: 'Vertex BVA Support Engagement',
      engagementType: EngagementType.BVA_SUPPORT,
      owner: 'Sascha Mueller',
      status: EngagementStatus.BLOCKED,
      strategicObjective: 'Move BVA workstream forward with updated customer inputs and value hypotheses.',
      summary: 'Used to track BVA-related activity and supporting collateral.',
      nextMilestone: 'BVA stakeholder alignment session',
      nextAction: 'Resolve missing customer metrics required for value model update',
      sourceEntryPath: 'Sales Cockpit / Vertex / BVA',
      meddpiccStatus: MeddpiccStatus.IN_PROGRESS,
      execPrepStatus: ExecPrepStatus.NOT_STARTED,
      accountId: 'SF-ACC-1003',
      accountName: 'Vertex Industrial',
      opportunityId: 'SF-OPP-2003',
      opportunityName: 'Vertex Transformation Program',
      bvaStatus: 'Blocked',
      bvaPhase: 'Model update',
      bvaLastUpdatedAt: new Date('2026-06-20T09:30:00.000Z'),
      artifacts: {
        create: [
          {
            artifactType: ArtifactType.SHEET,
            name: 'Value Model Inputs',
            path: 'https://example.com/sheets/vertex-value-model-inputs'
          },
          {
            artifactType: ArtifactType.DOC,
            name: 'Customer Discovery Summary',
            path: 'https://example.com/docs/vertex-discovery-summary'
          },
          {
            artifactType: ArtifactType.LINK,
            name: 'Supporting Internal Notes',
            path: 'https://example.com/links/vertex-internal-notes'
          }
        ]
      },
      externalRefs: {
        create: [
          {
            systemType: ExternalSystemType.SALESFORCE_ACCOUNT,
            externalId: 'SF-ACC-1003',
            label: 'Salesforce Account',
            url: 'https://example.com/salesforce/account/SF-ACC-1003'
          },
          {
            systemType: ExternalSystemType.SALESFORCE_OPPORTUNITY,
            externalId: 'SF-OPP-2003',
            label: 'Salesforce Opportunity',
            url: 'https://example.com/salesforce/opportunity/SF-OPP-2003'
          },
          {
            systemType: ExternalSystemType.BVA_RECORD,
            externalId: 'BVA-3003',
            label: 'BVA Workspace',
            url: 'https://example.com/bva/BVA-3003'
          }
        ]
      }
    }
  })

  console.log('Seed complete')
  console.log({
    strategicDealId: strategicDeal.id,
    execBriefingId: execBriefing.id,
    bvaSupportId: bvaSupport.id
  })
}

main()
  .catch((error) => {
    console.error(error)
    process.exit(1)
  })
  .finally(async () => {
    await prisma.$disconnect()
  })
```

## Recommended package.json support
Add a seed command so you can run the script easily.

Example:

```json name=package.json
{
  "prisma": {
    "seed": "ts-node prisma/seed.ts"
  }
}
```

If your setup uses another runner such as `tsx`, adapt accordingly.

## How to run the seed
After migrations are applied, run:

```bash name=run-seed.sh
npx prisma db seed
```

## Recommended validation after seeding
After the seed runs:
- verify the database was populated
- confirm `/engagements` shows multiple scenarios
- confirm detail pages show artifacts and external references
- confirm blocked/in-progress/other statuses appear correctly

## Optional next improvement
Once the first seed works, you can later:
- add more owners
- add more varied statuses
- add a completed engagement
- add longer summaries to stress test layouts
- add edge-case records with missing optional fields

## Important prototype advice
Do not overcomplicate the seed script.
Its job is to help you:
- develop faster
- test UI states
- validate the workflow
- spot schema issues early

## Summary
A small but realistic seed script gives you a much better prototype experience.
It helps you evaluate:
- list screens
- detail screens
- edit flows
- artifacts
- external references
- portfolio summaries
