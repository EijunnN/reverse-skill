# CLAUDE.md

This repository is a **task skill router** for authorized security research and analysis workflows.

## On Any Task

`RULES.md` is the single source of truth. Read it before doing anything else — it defines the startup sequence, routing matrix, authorization record, execution principles, and tool registry.

## First-Run Setup

`skills/tool-index.md` is not included in fresh clones. Generate it before starting:

```bash
# macOS / Linux
bash skills/scripts/refresh-tool-index.sh

# Kali Linux
bash kali/scripts/refresh-tool-index.sh
```

See `README_AI.md` for the complete bootstrap sequence.
