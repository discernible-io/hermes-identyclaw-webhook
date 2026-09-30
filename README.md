# IdentyClaw signed webhooks for Hermes

Hermes **platform plugin** (`identyclaw-webhooks`) that serves RODiT-signed
`/hooks/wake` and `/hooks/agent` (Ed25519 verify via the auth sidecar).

Leaves Hermes HMAC `/webhooks/{route}` unchanged.

| Surface | Value |
|---------|-------|
| GitHub repo | `discernible-io/hermes-identyclaw-webhook` |
| Plugin id / install dir | `identyclaw-webhooks` |
| Skill / docs text | use the repo name + plugin id above |

Requires the host auth package ([hermes-identyclaw-auth](https://github.com/discernible-io/hermes-identyclaw-auth))
with a healthy sidecar on `IDENTYCLAW_AUTH_PORT` (default `9910`).

## Install (Hermes way)

```bash
export HERMES_HOME="${HERMES_HOME:-$HOME/.hermes}"
curl -fsS http://127.0.0.1:9910/health

hermes plugins install discernible-io/hermes-identyclaw-webhook --no-enable
hermes plugins enable identyclaw-webhooks
```

`optional_env` from `plugin.yaml` (`IDENTYCLAW_HOOKS_PORT`, `IDENTYCLAW_AUTH_PORT`,
`IDENTYCLAW_HOOKS_HOST`) is prompted on install when you want to set values.

## Full playbook

```bash
bash "$HERMES_HOME/hermes-identyclaw-auth/scripts/install-stock-hermes.sh"
```

## Env

| Variable | Default | Role |
|----------|---------|------|
| `IDENTYCLAW_HOOKS_PORT` | `9911` | Listen for `/hooks/*` |
| `IDENTYCLAW_HOOKS_HOST` | `127.0.0.1` | Bind host (`0.0.0.0` behind nginx) |
| `IDENTYCLAW_AUTH_PORT` | `9910` | Auth sidecar |

Outbound tool: `send_rodit_webhook`.
