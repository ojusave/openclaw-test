# Deploy OpenClaw on Render

> [!IMPORTANT]
> **View full deployment instructions in the [Render docs](https://render.com/docs/deploy-openclaw).**

This template defines a `render.yaml` file you can use to deploy [OpenClaw](https://github.com/openclaw/openclaw) on Render. It uses the official project's [container image](https://github.com/openclaw/openclaw/pkgs/container/openclaw).

By default, this template uses the `latest` tag. Override this by setting the `OPENCLAW_VERSION` environment variable to a specific version tag.

## Setup

1. Click **Use this template > Create a new repository** in the upper right to copy this template into your account as a new repo.

2. Follow the deployment instructions in the [Render docs](https://render.com/docs/deploy-openclaw).

## Authentication

1. On your first visit, the landing page prompts for your `OPENCLAW_GATEWAY_TOKEN`
2. Valid token sets a signed, HTTP-only cookie (30-day expiry)
3. Sessions persist across service restarts

**Security:**
- Gateway binds to loopback only (never directly exposed)
- Constant-time token comparison
- Rate limiting (5 attempts/minute per IP)
- Secure cookies (HTTPS only, `SameSite=Lax`)

## Customization

The blueprint uses instance type **`pro`** (~4 GB RAM) and `NODE_OPTIONS=--max-old-space-size=3072` because the OpenClaw gateway (Node) can exceed a ~1.5 GB heap under load; **Standard (~2 GB)** is not enough headroom for that heap plus the Go proxy and OS. The proxy sets a single `NODE_OPTIONS` on every `openclaw` subprocess so the limit is always applied.

To save cost, you can switch the service to **Standard** in the dashboard and lower `NODE_OPTIONS` (for example `--max-old-space-size=1792`), understanding that heavy workloads may OOM again.

**Control UI “origin not allowed”:** On every startup the proxy applies `gateway.controlUi.allowedOrigins` for your Render hostname (`RENDER_EXTERNAL_HOSTNAME` or `RENDER_EXTERNAL_URL`). If you use a **custom domain**, set env `OPENCLAW_CONTROL_UI_ALLOWED_ORIGINS` to a JSON array of origins (e.g. `["https://openclaw.example.com"]`).

Override the OpenClaw version with a build argument:

```bash
docker build --build-arg OPENCLAW_VERSION=2026.2.3 -t openclaw-render .
```
