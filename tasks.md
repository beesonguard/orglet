# Orglet - Implementation Tasks

This task list is organized into phases with explicit research tasks and decision points. Each phase builds on the previous one.

**Legend:**
- `[R]` = Research task (learning, no code)
- `[D]` = Decision point (pause and evaluate before proceeding)
- `[ ]` = Implementation task
- `[x]` = Completed

---

## Phase 0: Project Foundation

### 0.1 Open Source Setup
- [ ] [R] Research open source best practices for mobile apps
  - License options (MIT vs Apache 2.0 vs GPL)
  - What files are needed (LICENSE, CONTRIBUTING.md, CODE_OF_CONDUCT.md)
  - How to structure a public repo for contributions
- [ ] [D] **DECISION: Confirm MIT license** (recommended for maximum adoption)
- [ ] Create GitHub repository `orglet` under your account
- [ ] Add LICENSE file (MIT)
- [ ] Add initial README.md with project description and "coming soon" note
- [ ] Add .gitignore for React Native projects
- [ ] Add CONTRIBUTING.md (basic template, expand later)

### 0.2 Development Environment
- [ ] [R] Research React Native development setup for Android
  - What's needed: Node.js, Android Studio, SDK, emulator
  - Current best practices (Expo vs bare React Native?)
  - TypeScript setup
- [ ] [D] **DECISION: Expo or bare React Native?**
  - Expo: Easier setup, but may limit native module access
  - Bare: More control, but more complex setup
  - Research what isomorphic-git and file system access require
- [ ] Install Android Studio and configure emulator
- [ ] Install Node.js (LTS version)
- [ ] Document setup steps in `/opt/orglet/docs/dev-setup.md`

---

## Phase 1: Hello World & Architecture Spike

### 1.1 React Native Scaffolding
- [ ] [R] Read React Native getting started docs (use Context7)
- [ ] Initialize React Native project with TypeScript template
- [ ] Run "Hello World" on Android emulator
- [ ] Run "Hello World" on physical Android device
- [ ] Set up basic project structure per PRD architecture
- [ ] Document any gotchas in `docs/dev-notes.md`

### 1.2 Navigation Setup
- [ ] [R] Research react-navigation best practices
- [ ] Install react-navigation and dependencies
- [ ] Create basic navigation structure:
  - [ ] Home screen (placeholder)
  - [ ] Reader screen (placeholder)
  - [ ] Settings screen (placeholder)
- [ ] Test navigation flow works

### 1.3 Styling Foundation
- [ ] [R] Research styling options: nativewind vs styled-components vs StyleSheet
- [ ] [D] **DECISION: Styling approach**
- [ ] Set up chosen styling solution
- [ ] Create basic theme (colors, typography, spacing)
- [ ] Implement dark mode / light mode toggle
- [ ] Test theme switching works

---

## Phase 2: Org Parsing Spike

### 2.1 Parser Research
- [ ] [R] Research org-mode parser options:
  - [ ] uniorg (TypeScript, unified ecosystem)
  - [ ] org-mode-ast
  - [ ] orga
  - [ ] Others?
- [ ] [R] For each parser, evaluate:
  - [ ] Syntax coverage (headings, links, formatting)
  - [ ] Performance with large files (test with a real ebook .org)
  - [ ] TypeScript support
  - [ ] Active maintenance
  - [ ] Bundle size
- [ ] Document findings in `research/org-parsers.md`
- [ ] [D] **DECISION: Choose org parser**

### 2.2 Parser Integration
- [ ] Install chosen parser
- [ ] Create `services/orgParser.ts` wrapper
- [ ] Write function to parse org string to AST
- [ ] Write function to extract heading tree from AST
- [ ] Test with sample org files (simple, medium, complex)
- [ ] Test with one of your ebook org files
- [ ] Document any parsing limitations/edge cases

---

## Phase 3: Core Reader View

### 3.1 Heading Tree Component
- [ ] Create `HeadingBlock` component
  - [ ] Display heading text with proper indentation
  - [ ] Hide org stars (*), show visual hierarchy instead
  - [ ] Tap to expand/collapse
  - [ ] Animate expand/collapse (nice to have)
- [ ] Create recursive heading tree renderer
- [ ] Test with nested headings (5+ levels deep)

### 3.2 Content Rendering
- [ ] Create `ContentBlock` component for body text
- [ ] Render basic formatting:
  - [ ] Bold (*text* or **text**)
  - [ ] Italic (/text/)
  - [ ] Links [[url][description]]
  - [ ] Plain text paragraphs
- [ ] Handle link taps (open in browser)
- [ ] Test with various org content

### 3.3 Reader Screen
- [ ] Create `ReaderScreen` with:
  - [ ] Header (file name, back button)
  - [ ] Scrollable content area
  - [ ] Heading tree + content rendering
- [ ] Load hardcoded sample org file first
- [ ] Test scrolling performance with large file
- [ ] [D] **DECISION: Need virtualization?** (if performance is bad)

### 3.4 State Persistence
- [ ] [R] Research react-native-mmkv or AsyncStorage
- [ ] [D] **DECISION: Storage solution**
- [ ] Create `services/storageService.ts`
- [ ] Save expanded heading state per file
- [ ] Restore expanded state on file open
- [ ] Save scroll position per file
- [ ] Restore scroll position on file open
- [ ] Test: close app, reopen, state preserved

---

## Phase 4: Local File Management

### 4.1 File System Access
- [ ] [R] Research file system access in React Native
  - [ ] react-native-fs
  - [ ] expo-file-system (if using Expo)
- [ ] Install and configure file system library
- [ ] Create `services/fileService.ts`
- [ ] Function to list .org files in a directory
- [ ] Function to read file contents
- [ ] Function to get file metadata (size, modified date)

### 4.2 File Browser Screen
- [ ] Create `FileBrowserScreen`
- [ ] Display list of .org files
- [ ] Show file name and last modified
- [ ] Tap file to open in Reader
- [ ] Handle empty state (no files)

### 4.3 Recent Files
- [ ] Track recently opened files in storage
- [ ] Display recent files on Home screen
- [ ] "Continue reading" card for last opened file
- [ ] Clear recent files option in Settings

---

## Phase 5: Git Integration

### 5.1 Git Library Spike
- [ ] [R] Research isomorphic-git
  - [ ] Does it work in React Native?
  - [ ] What are the limitations?
  - [ ] How does authentication work?
  - [ ] Performance considerations
- [ ] [R] Research alternatives if isomorphic-git doesn't work
- [ ] Create proof-of-concept: clone a public repo
- [ ] Document findings in `research/git-integration.md`
- [ ] [D] **DECISION: Git library choice**

### 5.2 Repository Management
- [ ] Create `services/gitService.ts`
- [ ] Function to clone repository (public)
- [ ] Function to pull latest changes
- [ ] Function to list files in repo
- [ ] Store repo metadata (url, last synced, local path)

### 5.3 Authentication
- [ ] [R] Research GitHub personal access token flow
- [ ] [R] Research GitLab personal access token flow
- [ ] Create token input UI in repo setup
- [ ] Store token securely (research secure storage options)
- [ ] Test clone/pull with private repo
- [ ] Handle auth errors gracefully

### 5.4 Repository Setup Screen
- [ ] Create `RepoSetupScreen`
- [ ] Input for repository URL
- [ ] Detect GitHub vs GitLab from URL
- [ ] Optional: personal access token input
- [ ] "Clone" button with progress indicator
- [ ] Success/error feedback
- [ ] Navigate to file browser on success

### 5.5 Multi-Repository Support
- [ ] Store list of configured repositories
- [ ] Display repos on Home screen
- [ ] Switch between repos
- [ ] Remove repository option
- [ ] Per-repo sync status (last synced, sync errors)

---

## Phase 6: Search

### 6.1 In-File Search
- [ ] Create `SearchOverlay` component
- [ ] Search input with keyboard
- [ ] Search through current file content
- [ ] Display matching results with context
- [ ] Tap result to jump to location
- [ ] Auto-expand headings containing matches
- [ ] Highlight matches in content

### 6.2 Global Search (Nice to Have)
- [ ] Search across all files in repository
- [ ] Display results grouped by file
- [ ] Performance consideration: index or on-demand?
- [ ] [D] **DECISION: Include in MVP or defer?**

---

## Phase 7: Polish & Testing

### 7.1 Error Handling
- [ ] Network error handling (clone/pull fails)
- [ ] Parse error handling (malformed org file)
- [ ] File not found handling
- [ ] User-friendly error messages
- [ ] Retry mechanisms where appropriate

### 7.2 Loading States
- [ ] Clone/sync progress indicator
- [ ] File loading indicator
- [ ] Skeleton screens or spinners
- [ ] Pull-to-refresh on file browser

### 7.3 Settings Screen
- [ ] Theme toggle (light/dark/system)
- [ ] Font size slider
- [ ] Manage repositories link
- [ ] About section (version, links)
- [ ] Clear cache option

### 7.4 Testing
- [ ] [R] Research React Native testing approaches
- [ ] Set up basic test infrastructure
- [ ] Unit tests for org parser service
- [ ] Unit tests for storage service
- [ ] Manual testing checklist
- [ ] Test on multiple Android versions/devices

### 7.5 Performance
- [ ] Profile app with large org files
- [ ] Identify and fix performance bottlenecks
- [ ] Lazy loading for large files
- [ ] Memory usage optimization

---

## Phase 8: Release Preparation

### 8.1 App Store Research
- [ ] [R] Research Google Play Store submission process
- [ ] [R] What's needed: screenshots, description, icons, privacy policy
- [ ] [R] Review process timeline and requirements
- [ ] Document in `docs/play-store-checklist.md`

### 8.2 Branding & Assets
- [ ] Design app icon (or commission/generate)
- [ ] Create splash screen
- [ ] Take screenshots for store listing
- [ ] Write store description
- [ ] Create feature graphic

### 8.3 Legal
- [ ] Write privacy policy (app doesn't collect data, just needs statement)
- [ ] Confirm MIT license is properly applied
- [ ] Any other legal requirements for Play Store

### 8.4 Build & Submit
- [ ] Set up release build configuration
- [ ] Generate signed APK/AAB
- [ ] Create Google Play Developer account ($25 one-time)
- [ ] Submit app for review
- [ ] Respond to any review feedback
- [ ] **LAUNCH!**

---

## Phase 9: Post-Launch & v0.2 Planning

### 9.1 Launch Activities
- [ ] Announce on Reddit r/orgmode
- [ ] Post on Hacker News
- [ ] Tweet/post about launch
- [ ] Monitor reviews and feedback
- [ ] Fix critical bugs quickly

### 9.2 v0.2 Planning
- [ ] Gather user feedback
- [ ] Prioritize editing features
- [ ] Plan iOS port
- [ ] Design capture/quick notes feature
- [ ] Update PRD for v0.2

---

## Parking Lot (Ideas for Later)

- [ ] Table rendering
- [ ] Code block syntax highlighting
- [ ] Timestamp parsing and display
- [ ] Properties drawer display
- [ ] Agenda view
- [ ] TODO state cycling
- [ ] Tags display and filtering
- [ ] Attachments/images
- [ ] Export to PDF
- [ ] Widgets for Android home screen
- [ ] Apple Watch companion
- [ ] Tablet-optimized layout

---

*Last updated: 2024-12-17*
