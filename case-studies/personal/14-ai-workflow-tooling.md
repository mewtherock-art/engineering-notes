# 14 · Tooling for working with AI agents

## What I built
- **Token meter** (`claude-token-meter`, pure standard library): reads the local session logs and
  shows token use per model per day. No network, no API key.
- **Skill registry + validator + runtime parity** (`agent-cli-setup`, `agent-skills`): one governed
  source of skills, an installer that symlinks them for each CLI, and tests that catch drift between
  copies.
- **Custom sub-agents** for recurring roles (frontend, backend, data, QA, reviewer, security
  reviewer, requirements, architecture), each with the house rules embedded because a sub-agent
  starts with no memory of the conversation.
- **Browser bridge** (`aipass-harness`): an OpenAI-compatible local endpoint that forwards jobs to
  a logged-in browser tab, so a CLI can use a service that only has a web login.
- **Dotfiles / setup backups** with a secret scan before every push.

## Principles
- **Start with the smallest model that can do the job** (read/search with a small one, implementation
  with a mid one, strongest only for high-stakes or after a failure) and always verify the result myself.
- **Proportionate process:** the full requirements → design → QA → review chain is for risky or
  cross-stack changes; a five-line fix just gets done.
- **Rules live in one place and are swept everywhere** — duplicated rules silently go stale.

## A lesson in humility
I built a Codex/Claude orchestrator that passed all its checks and was never used. I shelved it and
kept the reusable parts (chat bot, WSL auto-start, secure config loader). The takeaway became
rule #1 in [how-i-work.md](../../how-i-work.md).
