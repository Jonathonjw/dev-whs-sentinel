---
name: railway-deploy
description: Use when deploying applications to Railway, configuring Railway services, managing environments, setting up databases, or troubleshooting Railway deployments. Invoke for Railway CLI commands, railway.toml config, environment variables, and service networking.
license: MIT
metadata:
  author: epsillon-dev
  version: "1.0.0"
  domain: devops
  triggers: Railway, deploy to Railway, Railway service, Railway environment, Railway database, railway.toml, Railway CLI, Railway MCP
  role: engineer
  scope: implementation
  output-format: code
  related-skills: devops-engineer, supabase-postgres-best-practices
---

# Railway Deploy

Deploy and manage applications on Railway — the primary hosting platform for Epsillon Media projects.

## Stack Context

- **Backend**: Node.js / Express APIs
- **Frontend**: Next.js (also deployable to Vercel — prefer Vercel for frontend)
- **Database**: Supabase (external) — Railway hosts the app, not the DB
- **Primary use**: Backend APIs, worker services, long-running processes

## When to Use This Skill

- Deploying a new service to Railway
- Configuring `railway.toml` for build/start commands
- Setting up environment variables across environments
- Connecting Railway services (internal networking)
- Debugging failed deployments or crashed services
- Setting up Railway cron jobs or workers
- Managing Railway CLI locally

## Core Concepts

### Service Types
| Type | Use For |
|------|---------|
| Web Service | APIs, Next.js apps (with exposed port) |
| Worker | Background jobs, queue processors |
| Cron | Scheduled tasks (Railway-native cron) |
| Database | PostgreSQL, Redis (prefer Supabase for main DB) |

### Environments
Railway uses `production` and `staging` environments. Always test in staging first.

## railway.toml Reference

```toml
[build]
builder = "NIXPACKS"
buildCommand = "npm run build"

[deploy]
startCommand = "npm start"
healthcheckPath = "/health"
healthcheckTimeout = 30
restartPolicyType = "ON_FAILURE"
restartPolicyMaxRetries = 3

[[services]]
name = "api"
```

## Railway CLI Quick Reference

```bash
# Install
npm install -g @railway/cli

# Auth
railway login

# Link project
railway link

# Deploy current directory
railway up

# Run command in Railway environment
railway run npm run migrate

# Open service in browser
railway open

# View logs
railway logs

# Set environment variable
railway variables set KEY=value

# List variables
railway variables

# Switch environment
railway environment staging
```

## Environment Variables

- Set via Railway dashboard or CLI: `railway variables set KEY=value`
- Never commit secrets — use Railway's variable groups for shared secrets
- Use `railway run` locally to inject Railway env vars into local commands

### Common Variables for Epsillon Stack
```
NODE_ENV=production
PORT=$PORT                    # Railway injects this automatically
DATABASE_URL=                 # Supabase connection string
SUPABASE_URL=
SUPABASE_SERVICE_ROLE_KEY=
STRIPE_SECRET_KEY=
STRIPE_WEBHOOK_SECRET=
```

## Internal Networking

Services in the same Railway project can communicate via internal hostnames:

```
http://api.railway.internal:3000
```

Prefer internal networking over public URLs for service-to-service calls.

## Health Checks

Always add a health endpoint — Railway uses it to verify deployment success:

```javascript
app.get('/health', (req, res) => {
  res.json({ status: 'ok', timestamp: new Date().toISOString() });
});
```

## Deployment Workflow

1. **Test locally** with `railway run npm start`
2. **Push to git** — Railway auto-deploys on push if connected
3. **Monitor logs** with `railway logs --tail`
4. **Verify health check** passes before marking deploy done
5. **Check environment** — confirm correct variables are set

## Troubleshooting

| Problem | Check |
|---------|-------|
| Build fails | `railway logs` for build output; check `buildCommand` in railway.toml |
| Service crashes on start | Missing env vars; wrong `startCommand`; port not bound to `process.env.PORT` |
| Health check timeout | `/health` route missing or slow; increase `healthcheckTimeout` |
| Service not reachable | Check `PORT` is `process.env.PORT`, not hardcoded |
| Out of memory | Upgrade Railway plan or optimize memory usage |

## Port Binding (Critical)

Railway injects `$PORT` — always bind to it:

```javascript
const PORT = process.env.PORT || 3000;
app.listen(PORT, '0.0.0.0', () => {
  console.log(`Server running on port ${PORT}`);
});
```

Never hardcode a port in production.

## Constraints

### MUST DO
- Bind to `process.env.PORT`
- Add `/health` endpoint
- Set all secrets via Railway variables (never in code)
- Test in staging before promoting to production
- Use `railway.toml` for reproducible builds

### MUST NOT DO
- Use Railway PostgreSQL as the main DB (use Supabase)
- Hardcode ports or secrets
- Deploy directly to production without staging verification
- Store sensitive keys in `railway.toml`
