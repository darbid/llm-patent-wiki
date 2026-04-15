# Git Setup

## Initialization

```bash
cd patents/
git init
git add -A
git commit -m "patent-wiki: initial scaffold"
```

## Auto-Commit Hook

The `hooks/hooks.json` PostToolUse hook auto-commits wiki/ and .raw/ changes after every Write or Edit operation:

```bash
[ -d .git ] && git add wiki/ .raw/ 2>/dev/null && \
(git diff --cached --quiet || git commit -m "patent-wiki: auto-commit $(date '+%Y-%m-%d %H:%M')") || true
```

This runs silently. Failed commits are swallowed.

## Manual Commit Convention

When committing manually, use this format:

```
patent-wiki: [operation] [description]
```

Examples:
- `patent-wiki: ingest US20260072718A1 pass-a`
- `patent-wiki: ingest US20260072718A1 pass-b (42 pages created)`
- `patent-wiki: lint report 2026-04-14`
- `patent-wiki: scaffold initial structure`
