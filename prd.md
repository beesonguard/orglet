# Orglet - Product Requirements Document

## Overview

**Orglet** is a mobile org-mode file viewer designed for reading, navigating, and (eventually) editing `.org` files on iOS and Android. The key differentiator is **native Git integration** (GitHub, GitLab) rather than relying solely on cloud storage providers.

### Vision

Make org-mode accessible to people who don't want to learn Emacs. Provide a polished reading experience for org files, particularly for users who:
- Use org files as their GTD/productivity system
- Convert ebooks to org format for better reading/navigation
- Want to access their git-synced org files on mobile

### Business Model

- **Free tier:** Full viewer functionality
- **Paid tier ($5 one-time):** Editing and capture features

---

## Problem Statement

Current org-mode mobile apps are inadequate:

| App | Platform | Price | Issues |
|-----|----------|-------|--------|
| Orgzly | Android | Free | Abandoned, no Git support |
| Orgro | iOS/Android | $5 | No Git support, costs money to even view |
| MobileOrg | iOS/Android | Free | Dated UI, WebDAV/Dropbox only |
| beorg | iOS only | Subscription | No Android, recurring cost |

**The gap:** No quality **free** org viewer with Git-native sync exists.

---

## Target Users

### Primary: Org-mode enthusiasts
- Already use org files on desktop (Emacs, VS Code, etc.)
- Store files in Git repositories
- Want mobile access without complex sync setups

### Secondary: Curious newcomers
- Friends/colleagues of org users
- Want to try org-mode without Emacs learning curve
- Need a gentle on-ramp

### Tertiary: Ebook readers
- Convert books to org format for chapter navigation
- Want a reading app that remembers position via heading state

---

## MVP Scope (v0.1)

### In Scope

1. **Org File Viewing**
   - Parse and render org files with proper formatting
   - Collapsible headings (tap to expand/collapse)
   - Remember expanded state per file (acts as bookmark)
   - Support nested headings to arbitrary depth

2. **Git Integration (Read-Only)**
   - Clone public repositories (GitHub, GitLab)
   - Clone private repositories via personal access token
   - Pull latest changes
   - Browse repository file tree
   - Select which files/folders to sync

3. **Navigation & Search**
   - Quick search within current file
   - Jump to heading
   - Table of contents view (all headings)
   - Back/forward navigation history

4. **Reading Experience**
   - Clean, readable typography
   - Dark mode / light mode
   - Adjustable font size
   - Horizontal swipe between sibling headings (like pages)

5. **Local Storage**
   - Cache files offline
   - Track multiple repositories
   - Recently opened files

### Out of Scope for MVP

- Editing (v0.2+)
- Capture/quick notes (v0.2+)
- Git push (v0.2+)
- Agenda views (v0.3+)
- TODO state changes (v0.2+)
- Tables rendering (v0.2)
- Embedded images (v0.2)
- iOS (start with Android, add iOS in v0.2)

---

## User Stories

### Repository Setup
```
As a user
I want to connect my GitHub/GitLab repository
So that I can access my org files on mobile
```

**Acceptance Criteria:**
- Can authenticate via personal access token
- Can browse my repositories
- Can select specific folders to sync
- Can see sync status and last updated time

### File Reading
```
As a user
I want to read my org files with collapsible headings
So that I can navigate large documents easily
```

**Acceptance Criteria:**
- Headings collapse/expand on tap
- Collapsed state persists between sessions
- Nested headings indent visually
- Stars (*) are hidden, replaced by visual hierarchy

### Ebook Reading
```
As an ebook reader
I want Orglet to remember which chapter I'm on
So that I can pick up where I left off
```

**Acceptance Criteria:**
- Last opened file opens automatically
- Scroll position within expanded section is preserved
- "Continue reading" prompt on app open

### Search
```
As a user
I want to search within my org file
So that I can quickly find content
```

**Acceptance Criteria:**
- Search box accessible from reading view
- Results highlight matching text
- Tap result to jump to location
- Search expands collapsed headings containing matches

---

## Technical Architecture

### Platform Decision

**Recommendation: React Native**

| Option | Pros | Cons |
|--------|------|------|
| React Native | One codebase, large ecosystem, TypeScript, isomorphic-git works | Performance slightly lower than native |
| Flutter | Better performance, good UI toolkit | Dart learning curve, smaller ecosystem |
| Native | Best performance, platform-specific UX | 2x development effort |

React Native allows fastest path to MVP with acceptable performance for a document viewer.

### Key Dependencies

- **Org Parsing:** `uniorg` (TypeScript org-mode parser) or `org-mode-ast`
- **Git:** `isomorphic-git` (pure JS Git implementation)
- **Storage:** `react-native-mmkv` (fast key-value storage)
- **Navigation:** `react-navigation`
- **Styling:** `nativewind` (Tailwind for React Native)

### Data Model

```typescript
interface Repository {
  id: string;
  name: string;
  url: string;
  provider: 'github' | 'gitlab' | 'custom';
  authToken?: string;
  syncedFolders: string[];
  lastSynced: Date;
}

interface OrgFile {
  id: string;
  repositoryId: string;
  path: string;
  content: string;
  expandedHeadings: string[]; // heading IDs that are expanded
  scrollPosition: number;
  lastOpened: Date;
}

interface UserPreferences {
  theme: 'light' | 'dark' | 'system';
  fontSize: number;
  defaultRepository?: string;
  lastOpenedFile?: string;
}
```

### App Structure

```
/src
  /components
    /reader
      HeadingBlock.tsx
      ContentBlock.tsx
      TableOfContents.tsx
      SearchOverlay.tsx
    /repository
      RepoList.tsx
      RepoSetup.tsx
      FileBrowser.tsx
    /common
      Header.tsx
      Button.tsx
  /screens
    HomeScreen.tsx
    ReaderScreen.tsx
    SettingsScreen.tsx
    RepoSetupScreen.tsx
  /services
    gitService.ts
    orgParser.ts
    storageService.ts
  /hooks
    useOrgFile.ts
    useRepository.ts
  /types
    index.ts
  App.tsx
```

---

## UI/UX Guidelines

### Design Principles

1. **Content-first:** Minimize chrome, maximize reading area
2. **Familiar gestures:** Tap to expand, swipe to navigate
3. **Fast:** App should feel instant, even with large files
4. **Forgiving:** Easy to undo, hard to lose data

### Key Screens

1. **Home**
   - List of repositories
   - Recently opened files
   - "Continue reading" card if applicable

2. **File Browser**
   - Tree view of synced folders
   - File icons for .org files
   - Last modified timestamps

3. **Reader**
   - Clean document view
   - Floating TOC button
   - Search icon in header
   - Minimal header (file name, back button)

4. **Settings**
   - Theme toggle
   - Font size slider
   - Manage repositories
   - About/version

---

## Success Metrics

### MVP Launch (v0.1)
- [ ] 100 downloads in first month
- [ ] 4+ star rating on Play Store
- [ ] <3 critical bugs reported

### Growth (v0.3+)
- [ ] 1,000 monthly active users
- [ ] 10% conversion to paid tier
- [ ] Featured in org-mode community (Reddit, HN)

---

## Risks & Mitigations

| Risk | Likelihood | Impact | Mitigation |
|------|------------|--------|------------|
| Org parsing edge cases | High | Medium | Start with subset of org syntax, expand over time |
| Git auth complexity | Medium | High | Clear setup wizard, support docs |
| Performance with large files | Medium | Medium | Virtual scrolling, lazy parsing |
| Competition from Orgro | Low | Medium | Focus on Git integration differentiator |

---

## Timeline Estimate

| Phase | Duration | Deliverable |
|-------|----------|-------------|
| Setup & Architecture | 1 week | Project scaffolding, CI/CD |
| Core Parser & Viewer | 2-3 weeks | Basic org rendering with folding |
| Git Integration | 2 weeks | Clone, pull, auth flow |
| Polish & Testing | 1-2 weeks | Bug fixes, performance |
| Play Store Release | 1 week | Store listing, screenshots |

**Total MVP:** 7-9 weeks (part-time/vibe coding pace)

---

## Decisions Made

1. **Android first** - Primary development platform, no $99/year Apple fee, easier testing
2. **Open source from day one** - MIT license, public GitHub repo (needs setup help)
3. **MVP org syntax:** Headings, body text, links, bold/italic. Defer tables, code blocks, timestamps
4. **Offline-first** with manual sync trigger
5. **Timeline:** Nights and weekends project, ~7-9 weeks estimated

## Project Philosophy

**This is a learning project.** The developer (Michael) is a security architect, not a mobile developer. This PRD acknowledges:

- Many technical decisions require research spikes before committing
- Best practices need to be discovered and documented as we go
- Decision points are built into the task list
- "Vibe coding" with AI assistance, but with discipline and proper architecture
- The goal is to ship something real AND learn mobile development

### Areas Requiring Research

| Area | Unknown | How We'll Learn |
|------|---------|-----------------|
| React Native | Setup, project structure, best practices | Spike task + Context7 docs |
| Org parsing | Which library? Performance? Edge cases? | Evaluate 2-3 options |
| isomorphic-git | Auth flows, performance, offline support | Build proof-of-concept |
| App Store | Submission process, screenshots, metadata | Research when ready |
| Open source | License choice, repo setup, contributing guide | Setup task with guidance |
| Testing | What to test, how, CI integration | Research best practices |
| State management | Context vs Redux vs Zustand | Decide during implementation |

### Decision Points

The tasks.md includes explicit "DECISION" checkpoints where we pause, evaluate options, and document choices before proceeding.

---

## Next Steps

1. [x] Answer open questions above
2. [ ] Set up GitHub repository and open source foundation
3. [ ] Set up React Native project with TypeScript
4. [ ] Evaluate org parsers (uniorg vs alternatives)
5. [ ] Design wireframes for key screens
6. [x] Create tasks.md with atomic implementation steps

---

## Appendix: Competitor Analysis

### Orgro (Primary Competitor)
- **Price:** $5 to use at all
- **Strengths:** Clean UI, open source, active development
- **Weaknesses:** No Git support, Dropbox/iCloud only, limited search, costs money upfront
- **Opportunity:** Git integration + FREE viewer is major differentiator

### Orgzly Revived
- **Strengths:** Feature-rich, Android native
- **Weaknesses:** Complex UI, sync issues, no iOS
- **Opportunity:** Simpler UX, cross-platform

---

*Document Version: 0.1*
*Last Updated: 2024-12-17*
*Author: Michael + Claude*
