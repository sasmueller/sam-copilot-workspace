# SAM Copilot Workspace

A unified AI-powered workspace for Strategic Account Managers to drive customer strategy, quantify business value, manage deal qualification, prepare executives, and coordinate account growth.

## Product vision

SAM Copilot Workspace is designed to bring the core operating motions of a Strategic Account Manager into a single platform. Instead of spreading work across slide decks, CRM notes, BI dashboards, spreadsheets, and disconnected documents, the workspace provides one AI-assisted environment for account planning and execution.

The platform is intended to support:
- Business Value Assessments (BVA)
- MEDDPICC management
- Salesforce integration
- Power BI integration
- Executive briefings
- Account planning
- Competitive intelligence

## Architecture overview

The repository is organized as a modern TypeScript monorepo with clear boundaries between user-facing applications, backend services, shared packages, and documentation.

### Planned layers

- **apps/** — Next.js applications for SAM-facing workflows and dashboards
- **services/** — backend and AI orchestration services for data processing, scoring, simulation, and connectors
- **packages/** — shared TypeScript types, utilities, UI primitives, and SDKs
- **docs/** — product, architecture, roadmap, and implementation guidance
- **.github/** — GitHub Actions workflows and issue templates

### Initial platform shape

- **Frontend:** Next.js + Tailwind workspace for assessments, planning, and executive views
- **AI services:** recommendation, summarization, gap analysis, and simulation services
- **Data connectors:** Salesforce and Power BI adapters for account and performance context
- **Shared domain model:** reusable types for assessments, value mapping, and qualification workflows
- **Documentation-first delivery:** architecture and requirements live alongside implementation

## Planned modules

### 1. Business Value Assessment (BVA)
- Assessment creation and scoring
- Value hypothesis tracking
- Stakeholder-aligned business outcomes
- Baseline vs target state comparison

### 2. MEDDPICC workspace
- Metrics, Economic Buyer, Decision Criteria, Decision Process
- Paper Process, Identify Pain, Champion, Competition
- Deal inspection and qualification health

### 3. Salesforce integration
- Account, opportunity, and contact synchronization
- CRM context enrichment for planning and assessments
- Push/pull workflows for status and field updates

### 4. Power BI integration
- Business performance dashboard embedding
- KPI ingestion for assessment evidence
- Executive reporting and trend visualization

### 5. Executive briefings
- AI-assisted briefing generation
- Account summaries, risk flags, and opportunity narratives
- Output optimized for customer meetings and internal leadership reviews

### 6. Account planning
- Strategic goals, whitespace analysis, growth plans, and action tracking
- Team collaboration around priorities and engagement motion

### 7. Competitive intelligence
- Competitor profiles and threat tracking
- Differentiation mapping
- AI-generated battlecard support

## Repository structure

```text
.github/
  ISSUE_TEMPLATE/
  workflows/
apps/
services/
packages/
  shared-types/
docs/
  architecture/
```

## Setup instructions

### Prerequisites
- Node.js 20+
- npm 10+ (or compatible package manager)
- Git

### Initial setup

```bash
git clone https://github.com/sasmueller/sam-copilot-workspace.git
cd sam-copilot-workspace
npm install
```

### Recommended next steps

```bash
# initialize the main web app
mkdir -p apps/web

# initialize backend services
mkdir -p services/ai-orchestrator services/connectors

# validate TypeScript package structure
mkdir -p packages/shared-types/src
```

### Suggested future workspace setup
A future iteration of this repository should add:
- root `package.json`
- TypeScript project references
- workspace configuration (`npm`, `pnpm`, or `turbo`)
- Next.js app bootstrap under `apps/web`
- linting and formatting standards
- test runners and CI quality gates

## Development principles

- TypeScript-first domain modeling
- Monorepo-friendly package boundaries
- Docs-driven platform design
- GitHub Actions-based automation
- Integration-ready architecture for enterprise systems

## Contributing

This repository starts as a strategic platform scaffold. As implementation begins, contributors should use issue templates, roadmap guidance, and architecture docs to keep work aligned with the product vision.
