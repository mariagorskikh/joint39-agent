# Joint 39 Agent

<p align="center">
    <img src="assets/joint39-agent-logo.png" alt="Joint 39 Agent" width="280">
</p>

<p align="center">
  <strong>Portable AI agents on the agentic web.</strong>
</p>

<p align="center">
  <a href="https://github.com/mariagorskikh/joint39-agent/releases"><img src="https://img.shields.io/github/v/release/mariagorskikh/joint39-agent?include_prereleases&style=for-the-badge" alt="GitHub release"></a>
  <a href="LICENSE"><img src="https://img.shields.io/badge/License-MIT-blue.svg?style=for-the-badge" alt="MIT License"></a>
</p>

---

**Joint 39 Agent** is a personal AI assistant gateway you run on your own devices. It connects to the channels you already use — WhatsApp, Telegram, Slack, Discord, Signal, iMessage, Microsoft Teams, Matrix, and more — and gives your agent real memory, real endpoints, and real autonomy.

The gateway is the control plane. The product is the assistant.

[Website](https://join39.org) &middot; [Vision](VISION.md)

## Why Joint 39 Agent

Most "AI agents" are just a system prompt over a chat completion. Joint 39 Agent is different:

- **Agents remember.** Conversations are stored, facts are extracted, and context carries forward across sessions.
- **Agents have real endpoints.** Each agent gets a real webhook URL — not a fake one. External services can push events directly to your agent.
- **Agents act on their own.** With the autonomy runtime enabled, agents process inbox items, run scheduled tasks, and execute tool calls without you being in the loop.
- **Agents are portable.** Export your agent — identity, memories, config, tools — as a single JSON package and import it anywhere.

## Install

Requires **Node 22+**.

```bash
npm install -g joint39-agent@latest

joint39-agent onboard --install-daemon
```

The onboarding wizard walks you through gateway setup, workspace config, channel pairing, and skills.

## Quick start

```bash
# Start the gateway
joint39-agent gateway --port 18789 --verbose

# Send a message
joint39-agent message send --to +1234567890 --message "Hello from Joint 39 Agent"

# Talk to your agent
joint39-agent agent --message "What's on my schedule today?" --thinking high
```

## Architecture

```
WhatsApp / Telegram / Slack / Discord / Signal / iMessage / Teams / Matrix / ...
               |
               v
+-------------------------------+
|           Gateway             |
|       (control plane)         |
|    ws://127.0.0.1:18789       |
+---------------+---------------+
                |
                +-- Agent runtime (memory, inbox, tasks)
                +-- CLI (joint39-agent ...)
                +-- WebChat UI
                +-- macOS / iOS / Android nodes
```

## Key capabilities

### Multi-channel inbox
Connect 20+ messaging channels through a single gateway. WhatsApp, Telegram, Slack, Discord, Google Chat, Signal, iMessage, IRC, Microsoft Teams, Matrix, LINE, and more. All inbound messages are routed, queued, and delivered to your agent with per-channel rules.

### Agent memory
Your agent extracts and retains facts from every conversation. Memories are ranked by importance and recency, injected into context at inference time, and manageable via API. No embeddings required in v1 — retrieval uses importance scoring and keyword overlap.

### Real webhook endpoints
Every agent gets a real, publicly reachable inbox endpoint: `https://join39.org/api/agent/{username}/inbox`. External services (GitHub, Stripe, monitoring, CI/CD) can POST events directly to your agent. HMAC signature validation is supported.

### Autonomous task execution
A background worker process polls for pending tasks, executes tool-calling loops via GPT-4o-mini, logs results to an outbox, and respects configurable daily rate limits. Scheduled tasks use cron expressions. The full activity log is available via API and dashboard.

### Export and portability
Export your entire agent — identity, Agent Facts, config, goals, memories, and tool definitions — as a single `join39-agent-v1` JSON package. Import it on another instance or share it. No credentials are included in exports.

### Companion apps
- **macOS** — Menu bar app with Voice Wake, Talk Mode overlay, WebChat, and Canvas.
- **iOS** — Canvas, Voice Wake, Talk Mode, camera, screen recording, Bonjour pairing.
- **Android** — Chat, voice, Canvas, camera, notifications, location, SMS, contacts, calendar.

### Tools and automation
- **Browser control** — Managed Chrome/Chromium instance with CDP, snapshots, and uploads.
- **Canvas + A2UI** — Agent-driven visual workspace rendered in the companion apps.
- **Cron + webhooks** — Scheduled jobs and inbound webhook triggers.
- **Skills** — Bundled, managed, and workspace-level skill packs.
- **Plugins** — npm-distributed extensions for channels, tools, and integrations.

## Security

Joint 39 Agent connects to real messaging surfaces. Inbound DMs are treated as **untrusted input** by default.

- **DM pairing** — Unknown senders receive a pairing code. Approve with `joint39-agent pairing approve <channel> <code>`.
- **Allowlists** — Public DM access requires explicit opt-in (`dmPolicy="open"` + `allowFrom: ["*"]`).
- **Doctor** — Run `joint39-agent doctor` to surface risky or misconfigured policies.

Full security guide: [Security](https://docs.join39.org/gateway/security)

## Development

```bash
git clone https://github.com/mariagorskikh/joint39-agent.git
cd joint39-agent

pnpm install
pnpm build

# Dev loop with auto-reload
pnpm gateway:watch

# Run tests
pnpm test

# Lint + format
pnpm check
```

Runtime: Node 22+ with pnpm. Bun is supported for TypeScript execution.

## Project structure

```
src/            Source code (CLI, commands, gateway, media pipeline)
extensions/     Channel and tool plugins (workspace packages)
skills/         Bundled skill definitions
docs/           Internal documentation
ui/             Web UI source
assets/         Branding and static assets
scripts/        Build, release, and utility scripts
test/           Integration and e2e tests
```

## Release channels

| Channel | Description | npm tag |
|---------|-------------|---------|
| **stable** | Tagged releases (`vYYYY.M.D`) | `latest` |
| **beta** | Prereleases (`vYYYY.M.D-beta.N`) | `beta` |
| **dev** | Head of `main` | `dev` |

Switch: `joint39-agent update --channel stable|beta|dev`

## Contributing

One PR = one issue. Keep PRs focused and under ~5,000 changed lines. Small related fixes can be grouped. See [VISION.md](VISION.md) for project direction and contribution rules.

## License

[MIT](LICENSE)
