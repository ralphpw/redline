# Redline — Technical Architecture

## Overview

Redline is a desktop/web application that provides:
1. A file browser connected to a local Git repository
2. A chat interface for AI interaction
3. A redline (diff) view for reviewing changes
4. Simplified Git operations (commit, history, revert)

---

## Architecture Options

### Option A: Electron App
```
┌─────────────────────────────────────────┐
│            Electron Shell               │
│  ┌─────────────────────────────────┐   │
│  │         React Frontend          │   │
│  │  • File browser                 │   │
│  │  • Editor (Monaco/CodeMirror)   │   │
│  │  • Chat panel                   │   │
│  │  • Diff viewer                  │   │
│  └─────────────────────────────────┘   │
│  ┌─────────────────────────────────┐   │
│  │         Node.js Backend         │   │
│  │  • Git operations (isomorphic-git)│  │
│  │  • File system access           │   │
│  │  • LLM API calls                │   │
│  └─────────────────────────────────┘   │
└─────────────────────────────────────────┘
```

**Pros:** Full filesystem access, offline-capable, native Git  
**Cons:** Desktop-only, larger bundle, update distribution

### Option B: VS Code Extension
```
┌─────────────────────────────────────────┐
│              VS Code                    │
│  ┌─────────────────────────────────┐   │
│  │    Redline Sidebar Panel        │   │
│  │  • Chat interface               │   │
│  │  • Quick actions                │   │
│  └─────────────────────────────────┘   │
│  ┌─────────────────────────────────┐   │
│  │    Redline Diff View            │   │
│  │  • Custom diff renderer         │   │
│  │  • Approve/reject buttons       │   │
│  └─────────────────────────────────┘   │
│  ┌─────────────────────────────────┐   │
│  │    VS Code Git Integration      │   │
│  │  • Leverage built-in SCM        │   │
│  └─────────────────────────────────┘   │
└─────────────────────────────────────────┘
```

**Pros:** Leverage VS Code ecosystem, existing Git UI, fast to build  
**Cons:** Requires VS Code, still technical UX

### Option C: Web App + Local Agent
```
┌─────────────────────────────────────────┐
│            Browser (Web App)            │
│  • React frontend                       │
│  • Chat, diff view, file browser        │
└────────────────┬────────────────────────┘
                 │ WebSocket
┌────────────────▼────────────────────────┐
│         Local Agent (Node.js)           │
│  • Runs on user's machine               │
│  • Git operations                       │
│  • File system access                   │
│  • Proxies LLM calls                    │
└─────────────────────────────────────────┘
```

**Pros:** Web UX, local data stays local  
**Cons:** Two-part install, agent must be running

---

## Recommended: Start with VS Code Extension

**Rationale:**
1. Fastest to MVP — leverage existing editor, Git, diff views
2. Validate the workflow before building custom UI
3. Technical users can adopt immediately
4. Learnings inform standalone app later

**Migration path:**
VS Code Extension → Electron App (for non-technical users)

---

## Core Components

### 1. Chat Panel
- Input field for prompts
- Message history (user + AI)
- Context indicator ("AI can see: 12 files, 45KB")
- Action buttons: "Apply changes", "Review my edits"

### 2. Diff/Redline View
- Side-by-side or unified diff
- Syntax highlighting for Markdown
- Per-hunk approve/reject buttons
- Summary: "3 files changed, 12 insertions, 4 deletions"

### 3. File Browser
- Tree view of workspace
- File status indicators (modified, staged)
- Quick open/search

### 4. History View
- Timeline of commits
- Commit message + author
- Click to view diff at that point
- "Revert to this version" action

### 5. Git Abstraction Layer
```typescript
interface RedlineGit {
  // User-facing operations
  save(message: string): Promise<void>;      // = git add -A && git commit
  history(): Promise<Commit[]>;              // = git log
  undo(): Promise<void>;                     // = git revert HEAD
  diffWorking(): Promise<Diff>;              // = git diff
  diffCommit(sha: string): Promise<Diff>;    // = git show <sha>
  
  // Internal
  init(): Promise<void>;
  status(): Promise<FileStatus[]>;
}
```

### 6. AI Integration Layer
```typescript
interface RedlineAI {
  // Edit operations
  applyEdit(prompt: string, files: string[]): Promise<EditResult>;
  
  // Review operations  
  reviewChanges(diff: Diff): Promise<ReviewResult>;
  
  // Context
  getContext(): WorkspaceContext;
  setContextFiles(files: string[]): void;
}

interface EditResult {
  files: FileEdit[];
  explanation: string;
}

interface ReviewResult {
  issues: Issue[];
  suggestions: Suggestion[];
}
```

---

## Data Flow

### AI Edit Flow
```
User prompt
    │
    ▼
┌──────────────┐
│ Build context│ ← Selected files + workspace index
└──────┬───────┘
       │
       ▼
┌──────────────┐
│  LLM API     │ → Claude/GPT-4
└──────┬───────┘
       │
       ▼
┌──────────────┐
│ Parse edits  │ → Structured diff format
└──────┬───────┘
       │
       ▼
┌──────────────┐
│ Apply to     │ → Working directory (not committed)
│ working dir  │
└──────┬───────┘
       │
       ▼
┌──────────────┐
│ Show redline │ → User reviews
└──────┬───────┘
       │
       ▼
   Approve?
    │    │
   Yes   No
    │    │
    ▼    ▼
 Commit  Revert
```

### AI Review Flow
```
User makes manual edits
    │
    ▼
┌──────────────┐
│ Compute diff │ ← git diff
└──────┬───────┘
       │
       ▼
┌──────────────┐
│ Build context│ ← Diff + relevant files
└──────┬───────┘
       │
       ▼
┌──────────────┐
│  LLM API     │ → "Review these changes for issues"
└──────┬───────┘
       │
       ▼
┌──────────────┐
│ Show review  │ → Issues, suggestions, warnings
└──────────────┘
```

---

## Tech Stack (Recommended)

| Layer | Technology |
|-------|------------|
| **Frontend** | React + TypeScript |
| **Editor** | Monaco (VS Code's editor) or CodeMirror |
| **Diff rendering** | diff2html or custom |
| **Git** | isomorphic-git (pure JS) or simple-git (Node wrapper) |
| **LLM** | Anthropic Claude API / OpenAI API |
| **Desktop shell** | Electron (if standalone) |
| **State management** | Zustand or Redux Toolkit |

---

## Security Considerations

1. **API keys** — Stored in OS keychain, never in repo
2. **Local-first** — All data stays on user's machine
3. **No telemetry** — Or opt-in only
4. **LLM context** — User controls what files AI sees

---

## Next Steps

1. [ ] Decide: VS Code extension vs Electron vs Web+Agent
2. [ ] Prototype: Chat → LLM → Edit → Diff view
3. [ ] Git integration: Basic commit/history/revert
4. [ ] User testing with target persona

---

*Last updated: January 7, 2026*
