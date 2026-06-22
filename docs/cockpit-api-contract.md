# Cockpit API Contract

## Purpose
This document defines the MVP API contract for the Sales Cockpit web application.

The API supports:
- engagement creation and editing
- engagement list and detail retrieval
- artifact reference management
- read-only cross-system context exposure
- portfolio summary retrieval

This API contract follows the decisions in:
- `docs/mvp-decisions.md`
- `docs/cockpit-ui-blueprint.md`
- `docs/cockpit-data-model.md`
- `docs/sync-and-ownership-rules.md`

## API design principles
- the API exposes a normalized cockpit view model
- engagement workflow fields are first-class API resources
- CRM and BVA source data are exposed as read-only context in MVP
- artifact references are managed directly through the cockpit API
- reporting data is exposed simply in MVP and can later be expanded
- the frontend should not need to compose multi-system data itself

## Base assumptions
- API style: REST
- payload format: JSON
- auth model: to be chosen by implementation
- versioning: `/api/v1/...`
- timestamps: ISO 8601 UTC
- IDs: string-based identifiers

## Resource summary
Primary resources:
- Engagement
- Artifact Reference
- Portfolio Summary

Supporting embedded views:
- CRM Context
- BVA Summary
- Executive Prep Summary
- MEDDPICC Summary

---

## 1. Engagement endpoints

## GET `/api/v1/engagements`
Returns a list of engagements for the cockpit list page.

### Query parameters
- `status`
- `engagementType`
- `owner`
- `bvaStatus`
- `execPrepStatus`
- `search`
- `page`
- `pageSize`
- `sort`

### Response shape
```json name=engagements-list-response.json
{
  "items": [
    {
      "engagementId": "eng_123",
      "engagementName": "Acme Q4 Expansion",
      "engagementType": "opportunity-led",
      "engagementOwner": "user_1",
      "status": "active",
      "sourceEntryPath": "opportunity",
      "summary": "Expansion motion for enterprise rollout.",
      "nextMilestone": "Exec alignment call",
      "nextAction": "Confirm champion availability",
      "lastUpdatedDate": "2026-06-22T09:00:00Z",
      "account": {
        "accountId": "acct_1",
        "accountName": "Acme"
      },
      "opportunity": {
        "opportunityId": "opp_1",
        "opportunityName": "Acme Expansion FY26",
        "opportunityStage": "Proposal"
      },
      "bva": {
        "bvaId": "bva_1",
        "bvaStatus": "in-progress",
        "bvaPhase": "quantification",
        "bvaLastUpdated": "2026-06-21T12:00:00Z"
      },
      "meddpicc": {
        "meddpiccStatus": "active"
      },
      "execPrep": {
        "execPrepStatus": "planned",
        "execEventType": "ebr",
        "execEventDate": "2026-06-30"
      }
    }
  ],
  "page": 1,
  "pageSize": 25,
  "totalCount": 1
}
```

## GET `/api/v1/engagements/{engagementId}`
Returns the normalized detail view for an engagement.

### Response shape
```json name=engagement-detail-response.json
{
  "engagementId": "eng_123",
  "engagementName": "Acme Q4 Expansion",
  "engagementType": "opportunity-led",
  "engagementOwner": "user_1",
  "status": "active",
  "sourceEntryPath": "opportunity",
  "summary": "Expansion motion for enterprise rollout.",
  "strategicObjective": "Secure enterprise expansion with value-backed executive alignment.",
  "nextMilestone": "Exec alignment call",
  "nextAction": "Confirm champion availability",
  "createdDate": "2026-06-10T08:00:00Z",
  "lastUpdatedDate": "2026-06-22T09:00:00Z",
  "account": {
    "accountId": "acct_1",
    "accountName": "Acme",
    "industry": "Manufacturing",
    "segment": "Enterprise",
    "region": "NA",
    "salesforceUrl": "..."
  },
  "opportunity": {
    "opportunityId": "opp_1",
    "opportunityName": "Acme Expansion FY26",
    "opportunityStage": "Proposal",
    "opportunityOwner": "user_2",
    "salesforceUrl": "..."
  },
  "bva": {
    "bvaId": "bva_1",
    "bvaStatus": "in-progress",
    "bvaPhase": "quantification",
    "bvaLastUpdated": "2026-06-21T12:00:00Z",
    "bvaArtifactUrl": "..."
  },
  "meddpicc": {
    "meddpiccStatus": "active",
    "meddpiccSummaryArtifactUrl": "..."
  },
  "execPrep": {
    "execPrepStatus": "planned",
    "execEventType": "ebr",
    "execEventDate": "2026-06-30",
    "execBriefArtifactUrl": "..."
  },
  "artifacts": [
    {
      "artifactId": "art_1",
      "artifactType": "exec-brief",
      "artifactName": "Acme EBR Brief",
      "artifactPath": "...",
      "linkedEntityType": "engagement",
      "linkedEntityId": "eng_123"
    }
  ],
  "links": {
    "salesforceAccount": "...",
    "salesforceOpportunity": "...",
    "bvaSource": "..."
  }
}
```

## POST `/api/v1/engagements`
Creates an engagement.

### Request body
```json name=create-engagement-request.json
{
  "engagementName": "Acme Q4 Expansion",
  "engagementType": "opportunity-led",
  "engagementOwner": "user_1",
  "accountId": "acct_1",
  "opportunityId": "opp_1",
  "strategicObjective": "Secure enterprise expansion with value-backed executive alignment.",
  "nextMilestone": "Exec alignment call",
  "nextAction": "Confirm champion availability",
  "status": "active",
  "sourceEntryPath": "opportunity",
  "summary": "Expansion motion for enterprise rollout."
}
```

### Response
- `201 Created`
- returns created engagement detail view

## PATCH `/api/v1/engagements/{engagementId}`
Updates cockpit-owned engagement fields.

### Allowed writable fields
- `engagementName`
- `engagementType`
- `engagementOwner`
- `strategicObjective`
- `nextMilestone`
- `nextAction`
- `status`
- `sourceEntryPath`
- `summary`
- `meddpicc.meddpiccStatus`
- `execPrep.execPrepStatus`
- `execPrep.execEventType`
- `execPrep.execEventDate`

### Request body example
```json name=patch-engagement-request.json
{
  "nextMilestone": "Executive readout",
  "nextAction": "Finalize value messaging",
  "meddpicc": {
    "meddpiccStatus": "in-review"
  },
  "execPrep": {
    "execPrepStatus": "drafting",
    "execEventType": "ebr",
    "execEventDate": "2026-07-03"
  }
}
```

### Response
- `200 OK`
- returns updated engagement detail view

---

## 2. Artifact reference endpoints

## POST `/api/v1/engagements/{engagementId}/artifacts`
Creates a new artifact reference linked to an engagement.

### Request body
```json name=create-artifact-request.json
{
  "artifactType": "exec-brief",
  "artifactName": "Acme EBR Brief",
  "artifactPath": "https://github.com/example/repo/blob/main/docs/acme-ebr.md"
}
```

### Response
- `201 Created`
```json name=create-artifact-response.json
{
  "artifactId": "art_1",
  "artifactType": "exec-brief",
  "artifactName": "Acme EBR Brief",
  "artifactPath": "https://github.com/example/repo/blob/main/docs/acme-ebr.md",
  "linkedEntityType": "engagement",
  "linkedEntityId": "eng_123"
}
```

## DELETE `/api/v1/engagements/{engagementId}/artifacts/{artifactId}`
Deletes an artifact reference.

### Response
- `204 No Content`

---

## 3. Portfolio endpoints

## GET `/api/v1/portfolio/summary`
Returns the MVP portfolio summary.

### Response shape
```json name=portfolio-summary-response.json
{
  "activeEngagementCount": 42,
  "engagementsByType": {
    "account-led": 10,
    "opportunity-led": 22,
    "bva-led": 6,
    "executive-led": 4
  },
  "engagementsByStatus": {
    "active": 30,
    "on-hold": 7,
    "complete": 5
  },
  "bva": {
    "inProgressCount": 12,
    "completeCount": 8
  },
  "execPrep": {
    "upcomingCount": 9
  },
  "needsAttention": [
    {
      "engagementId": "eng_123",
      "engagementName": "Acme Q4 Expansion",
      "reason": "Exec prep planned but no brief linked"
    }
  ]
}
```

---

## 4. Validation rules

## Engagement create validation
Required:
- `engagementName`
- `engagementType`
- `engagementOwner`
- `accountId`
- `strategicObjective`
- `nextMilestone`
- `nextAction`
- `status`

Optional:
- `opportunityId`
- `summary`
- `sourceEntryPath`

## Enumerations
### `engagementType`
- `account-led`
- `opportunity-led`
- `bva-led`
- `executive-led`
- `simulation`

### `status`
- `not-started`
- `active`
- `on-hold`
- `complete`
- `archived`

### `meddpiccStatus`
- `not-started`
- `draft`
- `in-review`
- `active`
- `complete`

### `execPrepStatus`
- `not-planned`
- `planned`
- `drafting`
- `ready`
- `delivered`

### `execEventType`
- `customer-meeting`
- `qbr`
- `ebr`
- `internal-review`
- `board-level`
- `other`

### `artifactType`
- `account-plan`
- `opportunity-brief`
- `bva-discovery`
- `bva-model`
- `bva-executive-summary`
- `meddpicc-scorecard`
- `exec-brief`
- `portfolio-review`
- `stakeholder-map`
- `other`

---

## 5. Error model

### Standard error response
```json name=error-response.json
{
  "error": {
    "code": "validation_error",
    "message": "engagementName is required",
    "details": [
      {
        "field": "engagementName",
        "issue": "required"
      }
    ]
  }
}
```

### Suggested error codes
- `validation_error`
- `not_found`
- `forbidden`
- `unauthorized`
- `conflict`
- `upstream_unavailable`

---

## 6. MVP API boundaries

### API owns directly
- engagement CRUD
- cockpit-owned workflow fields
- artifact reference CRUD
- normalized view composition

### API does not own directly in MVP
- Salesforce master updates
- detailed BVA authoring
- Power BI reporting refresh
- artifact file storage

## Summary
The MVP Cockpit API should be a normalized orchestration API:
- direct for cockpit-owned workflow state
- read-only for CRM and BVA context
- simple for artifact references
- lightweight for portfolio summary
