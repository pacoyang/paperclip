# Paperclip - Phala Cloud Deployment

## Quick Start

### 1. Configure environment

```bash
cp .env.example .env
```

Edit `.env` and fill in the required values:

| Variable | Required | Description |
|----------|----------|-------------|
| `POSTGRES_PASSWORD` | Yes | PostgreSQL password |
| `BETTER_AUTH_SECRET` | Yes | Auth session signing secret |
| `PAPERCLIP_PUBLIC_URL` | Yes | Phala Cloud endpoint, e.g. `https://xxx.phala.cloud:3100` |
| `OPENAI_API_KEY` | No | OpenAI API key |
| `ANTHROPIC_API_KEY` | No | Anthropic API key |

### 2. Deploy

```bash
phala deploy --name paperclip --compose docker-compose.yml --instance-type tdx.large --disk-size 40 --image dstack-dev-0.5.8 --node-id 30 --env-file .env
```

### 3. Access

Open `https://<your-phala-endpoint>:3100` in your browser. On first visit, complete the bootstrap flow to create the initial admin account.

## Manage

```bash
# Check status
phala status --name paperclip

# View logs
phala logs --name paperclip

# Update
phala deploy --compose docker-compose.yml --env-file .env --cvm-id e51124b752c0d0f619d43a0b2c8051ce6907a2ba

# Remove
phala remove --name paperclip
```

## Services

| Service | Image | Port |
|---------|-------|------|
| paperclip | `ghcr.io/paperclipai/paperclip:latest` | 3100 |
| postgres | `postgres:17-alpine` | 5432 (internal) |

## Security Notes

- Deployment mode is `authenticated` — all access requires login or API key.
- Default exposure is `public`. To restrict by hostname, set `PAPERCLIP_DEPLOYMENT_EXPOSURE=private` and configure `PAPERCLIP_ALLOWED_HOSTNAMES`.
- To disable new user signup after initial setup, set `PAPERCLIP_AUTH_DISABLE_SIGN_UP=true`.
