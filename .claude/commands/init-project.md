Scan this project and auto-generate a customized CLAUDE.md file. Do NOT ask the user any questions — detect everything automatically.

## Step 1: Detect Tech Stack

Scan the project root for these files and infer the stack:

**Frontend:**
- `package.json` with react/next/vue/angular/svelte → detect framework
- `tsconfig.json` → TypeScript
- `tailwind.config.*` → TailwindCSS
- `postcss.config.*` / `sass` in dependencies → CSS preprocessor
- `vite.config.*` / `webpack.config.*` / `next.config.*` → build tool

**Backend:**
- `package.json` with express/fastify/koa/nestjs → Node.js framework
- `requirements.txt` / `pyproject.toml` / `Pipfile` → Python (detect django/flask/fastapi)
- `go.mod` → Go
- `Cargo.toml` → Rust
- `pom.xml` / `build.gradle` → Java/Kotlin
- `Gemfile` → Ruby/Rails
- `composer.json` → PHP/Laravel

**Database / BaaS:**
- `supabase/` directory or `@supabase/supabase-js` in deps → Supabase (PostgreSQL + Auth + Storage + Realtime)
- `firebase.json` / `@firebase` in deps → Firebase
- `.env` / `.env.example` → look for SUPABASE_URL, DATABASE_URL, DB_HOST, MONGO_URI, NEON_DATABASE_URL
- `docker-compose.yml` → check for postgres/mysql/mongo/redis services
- `prisma/schema.prisma` → Prisma + detect DB
- `knexfile.*` / `ormconfig.*` / `typeorm` in deps → ORM + detect DB
- `sequelize` in deps → Sequelize
- `drizzle.config.*` → Drizzle ORM
- Migration directories → detect DB type

**Hosting / Deployment:**
- `vercel.json` or `next.config.*` → Vercel
- `netlify.toml` → Netlify
- `fly.toml` → Fly.io
- `railway.toml` or `railway.json` → Railway
- `render.yaml` → Render
- `Dockerfile` → Docker (detect target platform from other configs)
- `docker-compose.yml` → Docker Compose
- `serverless.yml` / `serverless.ts` → Serverless Framework (AWS)
- `wrangler.toml` → Cloudflare Workers/Pages
- `app.yaml` → Google App Engine
- `Procfile` → Heroku / Railway

**CI/CD:**
- `.github/workflows/` → GitHub Actions
- `.gitlab-ci.yml` → GitLab CI
- `Jenkinsfile` → Jenkins
- `.circleci/config.yml` → CircleCI
- `terraform/` / `*.tf` → Terraform
- `k8s/` / `kubernetes/` → Kubernetes

**Testing:**
- `jest.config.*` / jest in deps → Jest
- `vitest` in deps → Vitest
- `cypress/` / `playwright/` → E2E framework
- `pytest` in deps → pytest
- `.mocharc.*` → Mocha

**Monorepo:**
- `pnpm-workspace.yaml` / `lerna.json` / `nx.json` / `turbo.json` → monorepo tool
- `packages/` / `apps/` directories → workspace structure

## Step 2: Generate CLAUDE.md

Write a `CLAUDE.md` file at the project root using the template from the existing CLAUDE.md, but replace the Tech Stack section with the auto-detected stack. Keep everything else (Delegation Table, Rules of Engagement, Workflows, etc.) exactly the same.

If the detected stack is very different from the default (e.g., Python instead of Node.js), also update the agent descriptions in the Delegation Table to reflect the actual technologies (e.g., "FastAPI backend" instead of "Express backend").

## Step 3: Report

Tell the user what was detected and that CLAUDE.md has been generated. List the detected technologies in a clean table.
