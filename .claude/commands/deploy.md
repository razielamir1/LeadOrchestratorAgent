Take this project from development to production. Execute the full deployment pipeline — do NOT ask the user to run commands manually.

## Pipeline

1. **@devops-engineer** — Assess the current state:
   - Read CLAUDE.md for the tech stack
   - Check if deployment is already configured (Dockerfile, vercel.json, fly.toml, railway.toml, etc.)
   - If NOT configured: recommend a platform, present the plan, and wait for PROCEED
   - If already configured: verify the configuration is complete

2. **@devops-engineer** — Run the deployment checklist:
   - Environment variables documented (.env.example)
   - Build command works
   - Health check endpoint exists
   - CI/CD pipeline configured
   - Flag anything missing and fix it

3. **@qa-expert** — Run a quick pre-deploy scan:
   - Run existing tests
   - Check for critical issues (unhandled errors, missing validation, exposed secrets)
   - If critical issues found: STOP and report before deploying

4. **@security-analyst** — Quick security check (in parallel with QA):
   - Scan for hardcoded secrets, exposed API keys
   - Check authentication/authorization basics
   - If critical vulnerabilities found: STOP and report

5. **Report to user:**
   - Summary of what was configured
   - Any manual steps required (e.g., "Go to Vercel dashboard and connect your GitHub repo")
   - Link to deployment docs if applicable

If $ARGUMENTS is provided, treat it as the target platform (e.g., `/deploy vercel`, `/deploy railway`, `/deploy supabase`).
