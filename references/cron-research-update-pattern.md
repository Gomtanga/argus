# Cron Job Pattern: Daily Research + Conditional Self-Update

This pattern runs Argus deep research daily on a schedule, evaluates whether the skill itself needs updating, and conditionally applies changes only when meaningful improvements are found.

## When to Use

- Any skill that should stay current with its domain
- Automated "eat your own dog food" workflows
- Scheduled research that feeds back into agent capabilities

## Setup

### 1. Pre-script: Current State Snapshot

Create a script at `~/.hermes/scripts/<name>.sh` that prints the current state (version, git log, working tree status, file size). This output is prepended to the cron job's context automatically.

Example (`argus-check-current.sh`):
```bash
#!/bin/bash
cd ~/repos/argus
echo "=== Git Log (local) ==="
git log --oneline -5
echo "=== Behind origin ==="
git log HEAD..origin/main --oneline
echo "=== Working Tree ==="
git status --short
echo "=== Version ==="
grep '\*\*Version:\*\*' SKILL.md
echo "=== File Size ==="
wc -c SKILL.md
echo "=== Last Modified ==="
git log -1 --format=%ad
```

### 2. Create the Cron Job

```bash
hermes cron create \
  --name "my-skill-daily-research" \
  --schedule "0 19 * * *" \
  --skill "my-skill" \
  --script "my-check-script.sh" \
  --workdir "/home/user/repos/my-skill" \
  --deliver "origin"
```

Key parameters:
- `--skill`: Pre-load the skill so the agent starts with full context
- `--script`: Pre-script output injected into prompt (use `~/.hermes/scripts/` path)
- `--workdir`: Git repo directory for file operations
- `--deliver "origin"`: Send results to the chat where cron was created

### 3. Prompt Structure

The prompt should follow this structure:

```
## Mission: [Skill Name] Deep Research & Self-Update

Research the latest trends in [topic domain], evaluate if [Skill Name] needs updating,
and apply changes conditionally.

### Workflow
1. Read current SKILL.md — understand current state
2. Phase A-C research on [topic domain] (3-7 searches/scrapes)
3. Compare findings against current SKILL.md sections
4. If meaningful improvements found → update SKILL.md, git commit, git push
5. If no meaningful improvements → report "no changes needed"
6. Sync Hermes skill if updated
```

### 4. Conditional Update Logic

```
UPDATE ONLY WHEN:
- New tool versions / commands available
- Framework methodology has evolved (new phases, new quality criteria)
- Community/industry best practices have shifted
- Bug or gap found in current implementation

DO NOT UPDATE FOR:
- Minor wording improvements
- Cosmetic formatting
- "It's been a while" — wait for real insight
```

### 5. Git Push with Token Auth

Since cron jobs run without interactive auth, use token-based push:

```bash
# Before push:
git remote set-url origin "https://gitlab-token:${TOKEN}@gitlab.example.com/user/repo.git"
git push origin main
# After push:
git remote set-url origin "https://gitlab.example.com/user/repo.git"
```

## Pitfalls

1. **Opaque tokens in URL**: The token-in-URL approach works but the token appears in shell history. Use env vars or git credential helpers for production.
2. **Frequency**: 1-2 updates per week is healthy. Daily updates for a stable skill is over-engineering.
3. **False positives**: The agent may want to change things just to report progress. The prompt must explicitly discourage unnecessary changes.
4. **Script path**: Pre-scripts must live under `~/.hermes/scripts/`. Use only the filename in `--script` — no absolute paths.
5. **Workdir**: Cron jobs start from a default directory. `--workdir` must be set to the repo root for git operations.
6. **Hermes skill sync**: If updating a GitLab repo's SKILL.md, also update the Hermes-installed version via `skill_manage(action='edit', ...)` so both stay in sync.
