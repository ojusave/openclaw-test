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

**Instance and Node heap:** The blueprint uses a **Pro** instance and `NODE_OPTIONS=--max-old-space-size=3072` in `render.yaml` so the gateway (Node) has enough heap alongside the Go proxy. You can switch to **Standard** and lower `NODE_OPTIONS` in the dashboard to reduce cost; heavy workloads may run out of memory on smaller instances.

**Control UI origins:** Each startup syncs `gateway.controlUi.allowedOrigins` for your Render hostname. If you use a **custom domain**, set `OPENCLAW_CONTROL_UI_ALLOWED_ORIGINS` to a JSON array of origins (for example `["https://your-domain.com"]`).

Override the OpenClaw version with a build argument:

```bash
docker build --build-arg OPENCLAW_VERSION=2026.2.3 -t openclaw-render .
```
