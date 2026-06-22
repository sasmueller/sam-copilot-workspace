# Cockpit Backend Schema

## Purpose
This document defines the MVP backend data schema for the Sales Cockpit application.

The schema supports:
- cockpit-owned engagement records
- cockpit-owned MEDDPICC summary state
- cockpit-owned executive prep summary state
- normalized artifact references
- external system references for Salesforce and BVA integration

This schema is designed for MVP simplicity and later extensibility.

## Schema design principles
- store cockpit-owned data directly
- reference external system identifiers rather than copying their full objects
- support normalized engagement retrieval
- separate artifact references from engagement records
- allow future extension without breaking MVP structure

## Suggested storage model
A relational model is recommended for MVP.

Core tables:
- `engagements`
- `engagement_artifacts`
- `engagement_external_refs`

Optional support tables later:
- `users`
- `accounts_cache`
- `opportunities_cache`
- `audit_events`

---

## 1. engagements table

### Purpose
Primary cockpit-owned operating record.

```sql name=engagements.sql
CREATE TABLE engagements (
  engagement_id VARCHAR(64) PRIMARY KEY,
  engagement_name VARCHAR(255) NOT NULL,
  engagement_type VARCHAR(32) NOT NULL,
  engagement_owner VARCHAR(64) NOT NULL,
  account_id VARCHAR(64) NOT NULL,
  opportunity_id VARCHAR(64) NULL,
  strategic_objective TEXT NOT NULL,
  next_milestone TEXT NOT NULL,
  next_action TEXT NOT NULL,
  status VARCHAR(32) NOT NULL,
  source_entry_path VARCHAR(32) NULL,
  summary TEXT NULL,

  meddpicc_status VARCHAR(32) NOT NULL,
  exec_prep_status VARCHAR(32) NOT NULL,
  exec_event_type VARCHAR(32) NULL,
  exec_event_date DATE NULL,

  created_at TIMESTAMP NOT NULL,
  updated_at TIMESTAMP NOT NULL
);
```

### Notes
This table contains all cockpit-owned editable workflow fields for MVP.

### Required indexes
```sql name=engagement-indexes.sql
CREATE INDEX idx_engagements_account_id ON engagements(account_id);
CREATE INDEX idx_engagements_opportunity_id ON engagements(opportunity_id);
CREATE INDEX idx_engagements_owner ON engagements(engagement_owner);
CREATE INDEX idx_engagements_status ON engagements(status);
CREATE INDEX idx_engagements_type ON engagements(engagement_type);
CREATE INDEX idx_engagements_updated_at ON engagements(updated_at);
```

---

## 2. engagement_external_refs table

### Purpose
Stores external-system summary references and read-only synchronized metadata.

```sql name=engagement_external_refs.sql
CREATE TABLE engagement_external_refs (
  engagement_id VARCHAR(64) PRIMARY KEY,
  bva_id VARCHAR(64) NULL,
  bva_status VARCHAR(32) NULL,
  bva_phase VARCHAR(64) NULL,
  bva_last_updated TIMESTAMP NULL,
  bva_artifact_url TEXT NULL,

  meddpicc_summary_artifact_url TEXT NULL,
  exec_brief_artifact_url TEXT NULL,

  salesforce_account_url TEXT NULL,
  salesforce_opportunity_url TEXT NULL,
  bva_source_url TEXT NULL,

  synced_at TIMESTAMP NULL,
  FOREIGN KEY (engagement_id) REFERENCES engagements(engagement_id)
);
```

### Notes
This table keeps external metadata separate from core cockpit-owned workflow fields.

---

## 3. engagement_artifacts table

### Purpose
Stores normalized artifact references linked to engagement records.

```sql name=engagement-artifacts.sql
CREATE TABLE engagement_artifacts (
  artifact_id VARCHAR(64) PRIMARY KEY,
  engagement_id VARCHAR(64) NOT NULL,
  artifact_type VARCHAR(64) NOT NULL,
  artifact_name VARCHAR(255) NOT NULL,
  artifact_path TEXT NOT NULL,
  artifact_owner VARCHAR(64) NULL,
  created_at TIMESTAMP NOT NULL,
  updated_at TIMESTAMP NOT NULL,
  FOREIGN KEY (engagement_id) REFERENCES engagements(engagement_id)
);
```

### Required indexes
```sql name=artifact-indexes.sql
CREATE INDEX idx_engagement_artifacts_engagement_id ON engagement_artifacts(engagement_id);
CREATE INDEX idx_engagement_artifacts_type ON engagement_artifacts(artifact_type);
```

---

## 4. Optional cache tables for MVP+
These are optional for MVP and can be introduced only if integration performance needs them.

## accounts_cache
```sql name=accounts-cache.sql
CREATE TABLE accounts_cache (
  account_id VARCHAR(64) PRIMARY KEY,
  account_name VARCHAR(255) NOT NULL,
  industry VARCHAR(128) NULL,
  segment VARCHAR(128) NULL,
  region VARCHAR(128) NULL,
  account_owner VARCHAR(64) NULL,
  synced_at TIMESTAMP NOT NULL
);
```

## opportunities_cache
```sql name=opportunities-cache.sql
CREATE TABLE opportunities_cache (
  opportunity_id VARCHAR(64) PRIMARY KEY,
  account_id VARCHAR(64) NOT NULL,
  opportunity_name VARCHAR(255) NOT NULL,
  opportunity_stage VARCHAR(64) NULL,
  amount DECIMAL(18,2) NULL,
  target_close_date DATE NULL,
  forecast_category VARCHAR(64) NULL,
  opportunity_owner VARCHAR(64) NULL,
  synced_at TIMESTAMP NOT NULL
);
```

### Recommendation
Do not add cache tables unless they are needed for:
- API latency control
- upstream reliability buffering
- reporting dataset generation

---

## 5. Suggested ORM/domain model

## Engagement entity
Fields:
- engagementId
- engagementName
- engagementType
- engagementOwner
- accountId
- opportunityId
- strategicObjective
- nextMilestone
- nextAction
- status
- sourceEntryPath
- summary
- meddpiccStatus
- execPrepStatus
- execEventType
- execEventDate
- createdAt
- updatedAt

## EngagementExternalRefs entity
Fields:
- engagementId
- bvaId
- bvaStatus
- bvaPhase
- bvaLastUpdated
- bvaArtifactUrl
- meddpiccSummaryArtifactUrl
- execBriefArtifactUrl
- salesforceAccountUrl
- salesforceOpportunityUrl
- bvaSourceUrl
- syncedAt

## EngagementArtifact entity
Fields:
- artifactId
- engagementId
- artifactType
- artifactName
- artifactPath
- artifactOwner
- createdAt
- updatedAt

---

## 6. Enumerations

## engagement_type
- `account-led`
- `opportunity-led`
- `bva-led`
- `executive-led`
- `simulation`

## status
- `not-started`
- `active`
- `on-hold`
- `complete`
- `archived`

## meddpicc_status
- `not-started`
- `draft`
- `in-review`
- `active`
- `complete`

## exec_prep_status
- `not-planned`
- `planned`
- `drafting`
- `ready`
- `delivered`

## exec_event_type
- `customer-meeting`
- `qbr`
- `ebr`
- `internal-review`
- `board-level`
- `other`

## artifact_type
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

## 7. Normalized read model strategy

### Engagement list read model
The list response should compose:
- `engagements`
- live or cached account display fields
- live or cached opportunity display fields
- `engagement_external_refs`

### Engagement detail read model
The detail response should compose:
- `engagements`
- `engagement_external_refs`
- `engagement_artifacts`
- live or cached CRM context
- BVA summary metadata

### Recommendation
Prefer composing a normalized application read model in the service layer rather than over-denormalizing the database too early.

---

## 8. Audit and change tracking
For strict MVP, audit can be minimal.

### Minimum recommendation
Store:
- `created_at`
- `updated_at`

### Recommended next step after MVP
Add:
- `audit_events`
- actor identity
- field-level change history

---

## 9. Portfolio summary derivation
Portfolio summary does not require a separate source-of-truth table in MVP.

It can be derived from:
- `engagements`
- `engagement_external_refs`
- optional reporting annotations later

### Recommendation
Do not create a `portfolio_snapshots` table in MVP unless needed for time-series historical analytics inside the app.

---

## 10. Migration strategy

### Phase 1
Create:
- `engagements`
- `engagement_external_refs`
- `engagement_artifacts`

### Phase 2
Add:
- cache tables only if needed

### Phase 3
Add:
- audit/event tables
- portfolio snapshot tables
- richer external-reference structures

## Summary
The MVP backend schema should be simple and durable:
- one core engagement table
- one external-reference table
- one artifact-reference table

That is enough to support:
- engagement editing
- cross-system summary display
- artifact linking
- portfolio summarization
