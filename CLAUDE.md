# CLAUDE.md - Orglet Project

## Project Overview

**Orglet** is a mobile org-mode file viewer for iOS and Android. The key differentiator is native Git integration (GitHub, GitLab) and a free viewer tier.

## Business Model

- **Free:** Full viewer functionality (read, navigate, search)
- **Paid ($5):** Editing and capture features

## Tech Stack

- **Framework:** React Native with TypeScript
- **Org Parsing:** TBD (evaluating uniorg, org-mode-ast)
- **Git:** isomorphic-git
- **Storage:** react-native-mmkv

## Directory Structure

```
/opt/orglet/
├── CLAUDE.md          # This file
├── prd.md             # Product requirements document
├── tasks.md           # Implementation tasks (to be created)
├── docs/              # Additional documentation
├── src/               # Source code (React Native app)
├── design/            # Wireframes, mockups
└── research/          # Competitor analysis, technical spikes
```

## Key Documents

- `prd.md` - Full product requirements, user stories, architecture decisions

## Development Commands

```bash
# To be added once project is scaffolded
```

## Current Status

- [x] Project created
- [x] PRD drafted
- [x] Open questions answered
- [x] tasks.md created with research phases and decision points
- [ ] Phase 0: Open source setup (GitHub repo, LICENSE)
- [ ] Phase 1: React Native scaffolding
- [ ] Phase 2: Org parsing spike

## Project Philosophy

This is a **learning project**. Michael is a security architect learning mobile development. The task list includes:
- Research tasks before committing to technical decisions
- Explicit decision points to pause and evaluate
- Documentation of learnings along the way
- "Vibe coding" with AI assistance, but with discipline

## Key Decisions

1. **Platform:** Android first (easier testing, no yearly fee)
2. **Framework:** React Native (fastest path to cross-platform MVP)
3. **Differentiators:** Free viewer + Git-native sync
4. **Open source:** MIT license, public GitHub repo from day one
5. **MVP syntax:** Headings, links, bold/italic (defer tables, code blocks)

## Competitors

| App | Our Advantage |
|-----|---------------|
| Orgro ($5) | Free viewer |
| Orgzly | Active development, Git support |
| MobileOrg | Modern UI, Git support |
| beorg | Android support, no subscription |
