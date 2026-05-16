# Changelog

All notable changes to dev-flow will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [3.0.0] - 2026-05-16

### Changed
- **Breaking**: Split single SKILL.md (553 lines) into layered architecture — index file (320 lines) + 7 independent flow files (`flows/A.md` through `flows/G.md`) loaded on demand
- Replaced trigger `工程方法论` (legacy name) with direct `dev-flow` trigger
- Clarified Flow C step mapping: steps 4-8 now explicitly reference corresponding Flow A steps

### Added
- Flow index table in SKILL.md with file paths and loading instructions
- Usage example section in README.md with end-to-end walkthrough
- CHANGELOG.md (this file)

## [2.0.0] - 2026-05-16

### Changed
- Renamed project from `engineering-methodology` to `dev-flow`
- Restructured from multi-skill collection to single-skill repository
- Rewrote SKILL.md with 7 flows, inline dual-path tool fallbacks, engineering principles, completion conditions, per-flow prohibitions, error handling table, and version bump rules
- Added user-invocable, triggers, and allowed-tools to frontmatter

## [1.0.0] - 2026-05-15

### Added
- Initial release as `engineering-methodology`
- 7-path decision tree for development task routing
- Basic flow definitions with tool suite integration
