# AGENTS.md

## Cursor Cloud specific instructions

This is a single-package TypeScript project: a Slack MCP (Model Context Protocol) server. Uses npm as its package manager (`package-lock.json`).

### Key commands

| Task | Command |
|------|---------|
| Install deps | `npm install` |
| Build | `npm run build` |
| Test | `npm test` |
| Watch mode | `npm run watch` |

### Runtime requirements

The server requires two environment variables to start: `SLACK_BOT_TOKEN` (a Slack Bot User OAuth Token starting with `xoxb-`) and `SLACK_TEAM_ID`. Without these, the process exits immediately. The `main()` function calls `initializeChannelCache()` on startup, which hits the Slack API — dummy tokens will cause an `invalid_auth` error and the process will exit before binding the HTTP port.

### Running the server locally

- **HTTP mode:** `SLACK_BOT_TOKEN=xoxb-... SLACK_TEAM_ID=T... node dist/index.js --transport http`
- **Stdio mode (default):** `SLACK_BOT_TOKEN=xoxb-... SLACK_TEAM_ID=T... node dist/index.js`
- Health check (HTTP mode only): `GET http://localhost:3000/health`

### Testing

Tests use Jest 30 with `ts-jest` ESM preset. All external dependencies (Slack API, MCP SDK, Express) are fully mocked — no real Slack credentials needed for tests. The `--experimental-vm-modules` flag is required (already configured in `package.json` scripts). There is no separate lint command configured in this project.
