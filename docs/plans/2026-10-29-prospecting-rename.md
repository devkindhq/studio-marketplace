# Rename `security` plugin → `prospecting`

**Date:** 2026-10-29
**Branch:** `rename/prospecting-plugin`
**Decision owner:** Ali Kazim

## Design note

The `security` plugin's real purpose is **prospecting**, not security work:

- **WhatWeb** (tech-detector skill) = lead qualification via technology fingerprinting —
  detect a prospect's stack to shape the pitch.
- **theHarvester** (harvester skill) = OSINT lead generation — emails, subdomains,
  employee names for outreach.

The `security` name misdescribes both skills and buries the sales use case. Renaming
the plugin to `prospecting` aligns the marketplace surface (install commands, README,
manifest) with the plugin's actual job.

### Non-goals

- Docker images `devkind/whatweb-mcp` and `devkind/theharvester-mcp` stay unchanged.
- `plugins/evidence-dev/**` untouched (separate extraction agent owns it).
- `.claude-plugin/marketplace.json` gets only the security→prospecting entry rename
  (no restructuring) to avoid merge conflicts with parallel agents.
- Standalone repos `devkindhq/whatweb-mcp` and `devkindhq/theharvester-mcp` are being
  created by other agents; this PR only links them as source repos in the README.

## Task plan

1. Branch `rename/prospecting-plugin` off `main`.
2. `git mv plugins/security plugins/prospecting` (history preserved).
3. `plugins/prospecting/.claude-plugin/plugin.json`: name → `prospecting`,
   description reframed to prospecting, keywords updated, version 1.2.1 → 1.3.0
   (CLAUDE.md: both version fields must be bumped or updates are skipped).
4. `plugins/prospecting/.mcp.json`: no change (image names unchanged).
5. Skills: `mcp__plugin_security_*` tool prefixes → `mcp__plugin_prospecting_*`;
   reframe descriptions that lead with "security".
6. `.claude-plugin/marketplace.json`: security entry → prospecting (name, source,
   description, version 1.3.0, category, keywords).
7. Root `README.md`: intro line, structure tree, prerequisites, plugin section
   (prospecting framing + source-repo links), install command, skill badge count.
8. `install.sh`: warning text + `claude plugin install prospecting@…`.
9. `CLAUDE.md`: `security/whatweb` → `prospecting/whatweb`.
10. `CHANGELOG.md`: 1.3.0 entry (existing format).
11. Grep whole repo for stale plugin references; leave unrelated uses of "security"
    (`SECURITY.md` policy, SEO skill content, Dockerfile upstream label, theHarvester
    source list) untouched.
12. Verify: both JSON files parse (`python -m json.tool`), no stale references, push
    branch, open PR (do not merge).

## Verification checklist

- [x] `git log --follow` shows history across the rename
- [x] `python -m json.tool` passes on marketplace.json and plugin.json
- [x] `grep -ri 'plugin_security\|security@'` returns nothing (excluding .git; the only remaining hits are this doc and the CHANGELOG entry describing the rename itself)
- [x] Badge counts match reality (3 plugins / 12 skills on disk — the old "11" badge was already stale before this rename; harvester was added without a bump)
- [x] PR open against `main`, not merged
- [ ] `CLAUDE.md:29` still says `security/whatweb` — edits to CLAUDE.md were blocked by an agent-file write-protection rule in this environment; flagged in the PR for the reviewer to apply by hand
