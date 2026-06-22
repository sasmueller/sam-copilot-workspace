# Step-by-Step Setup Guide

## Purpose
This guide explains how to start the Cockpit prototype in the simplest practical way.

It is written for someone who:
- is not a web frontend expert
- wants to avoid unnecessary complexity
- wants zero additional cost during the prototype phase
- wants a GitHub-native and accessible UI direction
- wants Microsoft Teams to be the primary access surface in practice

## Target prototype stack
Use:
- Next.js
- TypeScript
- Primer React
- Prisma
- SQLite
- Microsoft Teams as the intended primary access surface

## Development and access model
The recommended model is:
- develop locally in the browser first
- build a standard web app at the core
- treat Microsoft Teams as the intended primary user entry point

This means you should not block yourself on Teams-specific setup before the app works locally.
First make the app work well in a normal browser.
Then ensure the layout and flow are suitable for Teams embedding later.

## What you need before you start

### Required
- Git installed
- Node.js installed
- npm installed
- access to this GitHub repository
- a code editor such as VS Code

### Recommended
- terminal access on your machine
- basic familiarity with running commands in a shell
- access to Microsoft Teams for later validation of the usage model

## Step 1: Clone the repository
Open a terminal and run:

```bash name=clone-repo.sh
 git clone https://github.com/sasmueller/sam-copilot-workspace.git
 cd sam-copilot-workspace
```

## Step 2: Create the Next.js app structure
If the app does not exist yet, initialize it in the repository root.

Recommended command:

```bash name=create-next-app.sh
 npx create-next-app@latest . --typescript --eslint --src-dir --app --use-npm
```

### Notes
- choose **TypeScript**
- choose **App Router**
- choose **src directory**
- do not add unnecessary options if you are unsure

## Step 3: Start the app once
Run:

```bash name=run-app.sh
 npm install
 npm run dev
```

Then open the local URL shown in the terminal, usually:

```text name=local-url.txt
http://localhost:3000
```

### Goal
At this point, you should simply confirm that the app starts locally.

## Step 4: Install Primer React
In the project root, run:

```bash name=install-primer.sh
 npm install @primer/react @primer/octicons-react
```

### Goal
This gives you the GitHub-native React component foundation for the prototype UI.

## Step 5: Install Prisma and initialize it
Run:

```bash name=install-prisma.sh
 npm install prisma @prisma/client
 npx prisma init
```

### Goal
This creates the Prisma setup and the initial `prisma/` structure.

## Step 6: Configure SQLite
Open the `.env` file and set the database URL to SQLite.

Example:

```dotenv name=.env
DATABASE_URL="file:./dev.db"
```

### Goal
This keeps the prototype local and avoids any extra database service cost.

## Step 7: Update the Prisma schema
Open:

```text name=prisma-schema-path.txt
prisma/schema.prisma
```

Set the datasource provider to SQLite.

Example base structure:

```prisma name=schema.prisma
generator client {
  provider = "prisma-client-js"
}

datasource db {
  provider = "sqlite"
  url      = env("DATABASE_URL")
}
```

### Goal
You are preparing Prisma to generate a local SQLite-backed client.

## Step 8: Add the first prototype models
Add the initial models that match your planning docs:
- `Engagement`
- `EngagementExternalRef`
- `EngagementArtifact`

### Recommendation
Do not try to perfect the schema immediately.
Start with the MVP fields only.

## Step 9: Create the first migration
Run:

```bash name=prisma-migrate.sh
 npx prisma migrate dev --name init
```

### Goal
This creates the SQLite database and applies the first schema migration.

## Step 10: Generate the Prisma client
Run:

```bash name=prisma-generate.sh
 npx prisma generate
```

### Goal
This gives your app typed database access.

## Step 11: Create the initial app folders
Inside `src/`, create the first folders if they do not already exist:

```text name=initial-folders.txt
src/
├── app/
├── components/
├── features/
├── lib/
├── server/
└── types/
```

Recommended server subfolders:

```text name=server-folders.txt
src/server/
├── db/
├── services/
├── repositories/
├── validators/
└── integrations/
```

### Goal
Keep the code structure clear without overengineering.

## Step 12: Create the main routes
Start with these pages:

```text name=initial-routes.txt
src/app/
├── page.tsx
├── engagements/
│   ├── page.tsx
│   ├── new/
│   │   └── page.tsx
│   └── [engagementId]/
│       └── page.tsx
└── portfolio/
    └── page.tsx
```

### Goal
Create the visible product skeleton early.

## Step 13: Build a simple layout first
Create:
- a top navigation
- a page container
- a basic page title pattern

### Recommendation
Keep the first version very simple.
Do not try to perfect styling immediately.
Focus on clarity and structure.

## Step 14: Add a simple Primer test page
Use one route to verify Primer works.
For example, render:
- a heading
- a button
- a text input
- a card or boxed section

### Goal
Confirm that the UI stack is working before building product features.

## Step 15: Add a Prisma client helper
Create a shared database client module in something like:

```text name=prisma-client-path.txt
src/server/db/client.ts
```

### Goal
Keep database access centralized.

## Step 16: Build the first API route
Start with:

```text name=first-api-route.txt
src/app/api/v1/engagements/route.ts
```

### First implementation suggestion
Support:
- `GET` returning a simple list
- `POST` creating a simple engagement

### Goal
Get the first end-to-end workflow working as early as possible.

## Step 17: Seed sample data
Create a small set of realistic engagement records.

### Why this matters
A prototype is much easier to evaluate when:
- pages are not empty
- tables look realistic
- statuses and milestones can be seen in context

## Step 18: Build the engagement list page first
On `/engagements`, show:
- page title
- create button
- simple table
- a few status values

### Recommendation
Do this before building advanced detail editing.

## Step 19: Build the create page second
On `/engagements/new`, add:
- a basic form
- required fields only first
- submit behavior to the API

### Recommendation
Keep validation simple and clear.

## Step 20: Build the detail page third
On `/engagements/[engagementId]`, add:
- summary section
- editable core fields
- artifact section
- read-only context placeholders

### Recommendation
Use explicit save actions per section.
That is easier to understand and debug than autosave.

## Step 21: Validate layout assumptions for Teams
Before considering the prototype structurally sound, verify:
- the main pages remain readable at narrower widths
- important actions remain visible
- cards stack cleanly
- forms are still comfortable to use in constrained layout space

### Important note
You do not need full Teams packaging at this point.
Just ensure the UI will translate well into a Teams-hosted context.

## Step 22: Add the portfolio page last
The portfolio page can come after the core workflow works.

### Goal
Do not spend time on reporting before the engagement workflow is useful.

## Recommended implementation order summary
Build in this sequence:
1. app runs locally
2. Primer installed
3. Prisma + SQLite configured
4. initial schema created
5. first API route works
6. engagement list page works
7. create form works
8. detail page works
9. artifact handling works
10. Teams container suitability is checked
11. portfolio summary works

## Common mistakes to avoid

### Do not start with
- authentication
- deployment
- Salesforce integration
- Power BI integration
- advanced styling systems
- too many abstractions
- deep Teams-specific customization before the core workflow works

### Do start with
- local setup
- real sample data
- one working flow end to end
- accessible form and table behavior
- simple, clear UI structure
- layout choices that can work inside Teams later

## What success looks like
A good first success state is:
- the app runs locally
- the database works locally
- you can create an engagement
- you can see it in a list
- you can open a detail page
- the UI already feels clear and GitHub-native
- the layout looks suitable for eventual Teams-based use

## Next step after setup
Once the project skeleton works, the next best document to create would be a:
- first Prisma schema draft
- first API implementation checklist
- first UI scaffold checklist

## Summary
The best way to start this prototype is to keep it:
- local during development
- simple
- typed
- accessible
- GitHub-native
- Teams-aware
- focused on one useful workflow first
