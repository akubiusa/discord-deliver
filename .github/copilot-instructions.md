# Copilot review instructions

Development guidance lives in `AGENTS.md`; review against it.

## Language

Write review comments in English. Deliverables (code comments, docs, commits, PRs) are English; runtime messages are English.

## Review checklist

- No secrets (`DISCORD_TOKEN`, channel IDs) in code, logs, responses or commits.
- Error paths return the right HTTP status and a `{status, message}` JSON body, with `http_response_code()` before `exit`.
- Discord API status codes are still passed through; the `$http_response_header` status parsing is intact.
- Input validation is kept: JSON `Content-Type`, empty body, and a numeric-only channel ID in the URL.
- No new dependencies; `src/Dockerfile` `EXPOSE`, `entrypoint.sh` port and compose port mapping agree; linux/amd64 and linux/arm64 builds still work.
- Shell scripts pass ShellCheck (SC2086 and SC2001 are excluded in CI; do not flag them).
- Changes to endpoints, env vars or responses update `README.md` and `AGENTS.md`.
