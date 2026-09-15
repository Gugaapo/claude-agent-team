# Claude Code agent team

A versioned team of Claude Code subagents I run daily: 13 global specialists plus one
router, backed up as a portable tree so the team survives a machine rebuild or a move
between Windows, Linux and macOS.

## Why it looks like this

Subagents are only useful if the right one gets picked, with enough context, at the right
moment. So the team has three layers:

**1. `agent-manager` - the router.** A meta-agent that owns no domain of its own. It
analyses the request, discovers what is actually installed (both `~/.claude/agents/` and
the project-local `.claude/agents/`), then picks a routing strategy:

- single specialist, when one domain clearly owns the request
- sequential chain, when output feeds the next stage (design -> implement -> review)
- parallel consultation, when a decision needs competing perspectives

It also writes the delegation prompt: scope, constraints, relevant files, and what
"done" looks like, so the specialist does not have to re-derive intent. Everything it
routes on is documented, so the choice is inspectable rather than vibes.

**2. The 13 specialists.** Narrow jobs, explicit non-goals, each one tells the caller when
to use someone else instead:

| Agent | Owns |
|-------|------|
| `software-architect` | system design, boundaries, trade-off analysis |
| `data-pipeline-architect` | ingestion, transformation, scheduling, backfills |
| `linux-sysadmin-expert` | services, networking, storage, systemd |
| `dev-env-manager` | toolchains, reproducible local environments |
| `security-auditor` | threat modelling, authz/authn review, dependency risk |
| `performance-monitor` | profiling, bottleneck isolation, load behaviour |
| `git-operations-expert` | history surgery, branching strategy, recovery |
| `documentation-expert` | runbooks, references, onboarding material |
| `requirements-analyst` | turning vague asks into testable requirements |
| `prompt-architect` | prompt design, evaluation, model selection |
| `research-assistant` | sourced investigation and synthesis |
| `homelab-research-expert` | self-hosting, hardware, service selection |
| `pc-hardware-advisor-br` | build selection and pricing for the BR market |

**3. Lifecycle.** Nothing here is permanent. Two agents were retired once their scope was
absorbed by better-defined neighbours; an agent that does not visibly earn its place gets
merged or deleted, and `MANIFEST.yaml` records what is live.

## Layout

```
global-agents/           14 agent definitions (markdown frontmatter + system prompt)
scripts/                 backup + restore for Linux (bash) and Windows (PowerShell)
MANIFEST.yaml            what is live, with sizes and platforms
INSTALL.md               install / restore / update walkthrough
```

## Install

```bash
git clone https://github.com/Gugaapo/claude-agent-team.git
mkdir -p ~/.claude/agents
cp claude-agent-team/global-agents/*.md ~/.claude/agents/
```

Windows equivalent and restore steps: [INSTALL.md](INSTALL.md).

## Backups

`scripts/backup-agents-linux.sh` and `scripts/backup-agents-windows.ps1` copy global agents
plus each project-local `.claude/agents/` directory into this tree and commit. Line endings
and path handling are pinned in `.gitattributes` so the same repo restores correctly on
both platforms.

## Notes

- Agent definitions are markdown with YAML frontmatter (`name`, `description`, `model`,
  `tools`); the `description` field is what the router matches against, so it is written as
  a trigger, not a summary.
- Definitions that embed a specific codebase - project-local agent sets and the API-design
  specialist - stay in a private repo instead of this one.

## License

MIT - see [LICENSE](LICENSE).
