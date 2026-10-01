# IdentyClaw signed webhooks for Hermes

Hermes **platform plugin** (`identyclaw-webhooks`) that serves RODiT-signed
`/hooks/wake` and `/hooks/agent` (Ed25519 verify via the auth sidecar).

Leaves Hermes HMAC `/webhooks/{route}` unchanged.

Follows the stock [Hermes Plugins](https://hermes-agent.nousresearch.com/docs/user-guide/features/plugins/) flow:
`hermes plugins install owner/repo` → enable (opt-in).

| Surface | Value |
|---------|-------|
| GitHub repo | `discernible-io/hermes-identyclaw-webhook` |
| Plugin id / install dir | `identyclaw-webhooks` |

Requires the host auth package ([hermes-identyclaw-auth](https://github.com/discernible-io/hermes-identyclaw-auth))
with a healthy sidecar on `IDENTYCLAW_AUTH_PORT` (default `9910`).

Catalog submission is optional; `owner/repo` is enough.

## Install (default Hermes UX)

```bash
export HERMES_HOME="${HERMES_HOME:-$HOME/.hermes}"
curl -fsS http://127.0.0.1:9910/health

hermes plugins install discernible-io/hermes-identyclaw-webhook
# Enable 'identyclaw-webhooks' now? → y
```

Confirm with `hermes plugins list --plain --no-bundled`. Restart the gateway if it is already running.

### Scripted / non-interactive

```bash
hermes plugins install discernible-io/hermes-identyclaw-webhook --enable
# or: --no-enable  then  hermes plugins enable identyclaw-webhooks
```

`optional_env` from `plugin.yaml` (`IDENTYCLAW_HOOKS_PORT`, `IDENTYCLAW_AUTH_PORT`,
`IDENTYCLAW_HOOKS_HOST`) is prompted on interactive install when unset.

## Full playbook

```bash
bash "$HERMES_HOME/hermes-identyclaw-auth/scripts/install-stock-hermes.sh" \
  --a2a-public-url "https://YOUR.PUBLIC.HOST"
```

## Env

| Variable | Default | Role |
|----------|---------|------|
| `IDENTYCLAW_HOOKS_PORT` | `9911` | Listen for `/hooks/*` |
| `IDENTYCLAW_HOOKS_HOST` | `127.0.0.1` | Bind host (`0.0.0.0` behind nginx) |
| `IDENTYCLAW_AUTH_PORT` | `9910` | Auth sidecar |

Outbound tool: `send_rodit_webhook`.
