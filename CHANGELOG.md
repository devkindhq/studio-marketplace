# Changelog

## [1.3.0] - 2026-10-29
### Changed
- Renamed `security` plugin to `prospecting` — its real purpose is prospecting: lead qualification via WhatWeb tech fingerprinting and OSINT lead generation via theHarvester (per Ali's decision). Install command is now `claude plugin install prospecting@agency-marketplace`.
- prospecting: reframed plugin/skill descriptions and keywords around sales prospecting; category `security` → `sales`; version 1.2.1 → 1.3.0. MCP tool prefixes updated (`mcp__plugin_security_*` → `mcp__plugin_prospecting_*`). Docker images (`devkind/whatweb-mcp`, `devkind/theharvester-mcp`) unchanged.

## [1.0.1] - 2026-06-17
### Fixed
- evidence-dev: moved plugin.json to correct .claude-plugin/ location
- evidence-dev: removed skills array (skills are auto-discovered)

## [1.0.0] - 2026-05-23
### Added
- security plugin with WhatWeb and theHarvester MCP servers and tech-detector skill
- evidence-dev plugin with 9 Evidence.dev documentation skills
- seo plugin with search-quality-rater skill
- install.sh for one-command marketplace registration
