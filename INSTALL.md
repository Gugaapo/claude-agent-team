# Install / restore

## Linux, macOS, WSL

```bash
git clone https://github.com/Gugaapo/claude-agent-team.git
mkdir -p ~/.claude/agents
cp claude-agent-team/global-agents/*.md ~/.claude/agents/
```

## Windows (PowerShell)

```powershell
git clone https://github.com/Gugaapo/claude-agent-team.git
New-Item -ItemType Directory -Force -Path "$env:USERPROFILE\.claude\agents"
Copy-Item -Path "claude-agent-team\global-agents\*" -Destination "$env:USERPROFILE\.claude\agents\"
```

## Project-local agents

Project-local agents live in `<project>/.claude/agents/` and take precedence over the global
copy of the same name - that is how a project overrides the router or adds a specialist that
only makes sense in that codebase.

```bash
cd /path/to/your/project
mkdir -p .claude/agents
cp /path/to/agents/*.md .claude/agents/     # the ones that project needs, not all of them
```

They are intentionally not published in this repo (see the README).

## Verify

```bash
ls ~/.claude/agents/                 # 14 definitions
head -5 ~/.claude/agents/agent-manager.md   # frontmatter intact
```

In a session, ask `agent-manager` which agents it can see - it reports what it actually
discovered rather than what you think is installed.

## Updating

Edit the definition that is wrong, then push it back into this repo:

```bash
cp ~/.claude/agents/*.md /path/to/claude-agent-team/global-agents/
cd /path/to/claude-agent-team && git add -A && git commit -m "agents: <what changed>"
```

## Automated backup

```bash
./scripts/backup-agents-linux.sh          # Linux/macOS/WSL
.\scripts\backup-agents-windows.ps1      # Windows
```

Both scripts copy global agents, plus any project-local `.claude/agents/` directories you
list at the top of the script, into this tree and commit the change.
