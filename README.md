# Redline

> **"AI-powered document editing with redline review"**

A tool for human + AI peer collaboration on complex multi-document projects, with bi-directional diff review and full version history.

---

## The Problem

Knowledge workers (lawyers, BAs, compliance officers, technical writers) collaborate on complex document sets where:

1. **Context spans multiple files** — A contract suite, policy set, or spec where Section 3.2 of Doc A must align with Schedule B
2. **AI can help** — But current tools (ChatGPT, Copilot in Word) are single-doc, copy-paste, no history
3. **Trust requires reviewability** — You can't blindly accept AI output; you need to see *exactly* what changed
4. **Iteration is essential** — Documents evolve over weeks/months; you need to see "what changed since Tuesday?"

### Current Solutions Fall Short

| Tool | Version Control | Diff Quality | AI Editing | Multi-Doc Context |
|------|-----------------|--------------|------------|-------------------|
| **Word + Track Changes** | ✅ Built-in | ✅ Inline redlines | ❌ No AI in-place editing | ❌ Separate files |
| **Word + Git** | ✅ Commits exist | ❌ Binary blob | ❌ Same problem | ⚠️ No unified diff |
| **Google Docs + AI** | ⚠️ Version history | ✅ Suggestions | ⚠️ Chat sidebar only | ❌ Per-doc only |
| **GitHub Copilot Agent** | ✅ Full Git | ✅ Line-by-line | ✅ Agent edits | ✅ Workspace | 
| **GitBook Agent** | ⚠️ Platform-locked | ✅ Rich diff | ✅ Conversational | ⚠️ Docs only |

**The gap:** No general-purpose, non-technical-friendly tool where:
- AI has full workspace context
- AI applies structured edits in place
- Human reviews changes via diff before accepting
- AI can also review human's changes
- Full Git history without CLI complexity

---

## The Solution

### Core Concept

```
┌─────────────────────────────────────────────────────────────────┐
│                         WORKSPACE                               │
│  ┌─────────┐ ┌─────────┐ ┌─────────┐ ┌─────────┐ ┌─────────┐   │
│  │  .md    │ │  .docx  │ │  .pdf   │ │  .json  │ │  .txt   │   │
│  └─────────┘ └─────────┘ └─────────┘ └─────────┘ └─────────┘   │
│                         ▼                                       │
│              ┌─────────────────────┐                           │
│              │   SEARCH INDEX      │  ← semantic + keyword     │
│              └─────────────────────┘                           │
└─────────────────────────────────────────────────────────────────┘
                          │
         ┌────────────────┼────────────────┐
         ▼                ▼                ▼
   ┌──────────┐    ┌─────────────┐   ┌──────────┐
   │  HUMAN   │    │    CHAT     │   │    AI    │
   │  edits   │◄──►│  interface  │◄──►│  edits   │
   └──────────┘    └─────────────┘   └──────────┘
         │                                │
         ▼                                ▼
   ┌──────────┐                    ┌──────────┐
   │ REDLINE  │◄── AI reviews ────│ REDLINE  │
   │ (human)  │                    │  (AI)    │
   └──────────┘──── human reviews─►└──────────┘
         │                                │
         └────────────┬───────────────────┘
                      ▼
              ┌──────────────┐
              │    COMMIT    │  ← labeled, reversible
              └──────────────┘
                      │
                      ▼
              ┌──────────────┐
              │  GIT HISTORY │  ← full audit trail
              └──────────────┘
```

### Key Differentiators

| Feature | Why It Matters |
|---------|----------------|
| **Bi-directional redline review** | AI catches your inconsistencies; you catch AI hallucinations. Trust without blind acceptance. |
| **Multi-file workspace context** | AI sees *everything* — not just the file you're editing. Cross-doc consistency checks. |
| **Semantic + keyword search** | "Find everywhere we mention liability caps" across 50 docs |
| **File-type agnostic** | Markdown, Word, PDF (read), JSON, plain text — all in one project |
| **Non-technical Git** | No branches, no CLI, no merge conflicts. Just: edit → review → approve |
| **AI review mode** | "Check my changes for issues" — AI reviews YOUR redline, not just the other way around |

---

## Target Users

1. **Legal teams** — Contract suites, M&A due diligence, policy drafts
2. **Compliance officers** — SOC2 documentation, GDPR records, audit prep
3. **Business analysts** — Requirements specs, stakeholder interviews → structured docs
4. **Technical writers** — User guides, API docs, release notes
5. **HR/Operations** — Handbooks, SOPs, onboarding documentation
6. **Researchers** — Literature reviews, grant applications, thesis chapters
7. **Anyone drafting important emails** — See below

---

## Use Case: Email Drafting (The Broken Workflow)

### Current State (Garbage)

```
┌─────────┐    ┌─────────┐    ┌─────────┐    ┌─────────┐
│ Draft   │───►│ Paste   │───►│ AI edits│───►│ Paste   │
│ email   │    │ into AI │    │ ???     │    │ back    │
└─────────┘    └─────────┘    └─────────┘    └─────────┘
                                  │
                            What changed?
                            No idea. 🤷
```

**Problems:**
- **No diff** — You can't see what AI changed. Did it soften your tone? Add a commitment you didn't intend?
- **No history** — "What was my original draft?" Gone.
- **No context** — AI doesn't know the email thread, the project, the relationship
- **Round-trip friction** — Copy, paste, copy, paste, lose formatting
- **No iteration** — Each paste is a fresh start; no "undo that last change"

### With Redline

```
┌─────────────────────────────────────────────────────┐
│ EMAIL PROJECT: "Q1 Client Renewal Negotiation"     │
│                                                     │
│  📄 thread-context.md    (background, history)     │
│  📄 draft-v1.md          (your draft)              │
│  📄 draft-v2.md          (AI revision)             │
│  📄 final.md             (approved version)        │
└─────────────────────────────────────────────────────┘
                    │
        ┌───────────┴───────────┐
        ▼                       ▼
   ┌─────────┐            ┌─────────┐
   │ You:    │            │ AI:     │
   │ "Make   │───────────►│ Edits   │
   │ this    │            │ in      │
   │ firmer" │◄───────────│ place   │
   └─────────┘  REDLINE   └─────────┘
                  │
        "Changed 'I think' → 'I recommend'"
        "Added deadline in paragraph 2"
        "Removed hedge in closing"
                  │
              APPROVE / REJECT
```

**Benefits:**
- **See exactly what changed** — Redline shows every word AI touched
- **Full context** — AI sees the thread history, the project background
- **Iterate with history** — "Undo that last change" = revert commit
- **Reusable templates** — Save successful email patterns for similar situations
- **Audit trail** — "What did I actually send?" is always recoverable

---

## MVP Feature List

### Must Have (v0.1)
- [ ] Connect local folder (Git repo)
- [ ] File browser (tree view)
- [ ] Chat panel (prompt AI)
- [ ] AI applies edits in place (Markdown files first)
- [ ] Redline view (before/after, per-file)
- [ ] Approve/Reject per hunk
- [ ] Commit with message
- [ ] History view (timeline of commits)

### Should Have (v0.2)
- [ ] Search: keyword across all files
- [ ] Search: semantic ("find similar to this paragraph")
- [ ] AI review mode ("Check my changes")
- [ ] Multi-file edits in single operation
- [ ] Undo last AI action (revert commit)

### Nice to Have (v0.3+)
- [ ] Word/DOCX support (read/write)
- [ ] PDF support (read-only, extract text)
- [ ] Branching for "what if" exploration
- [ ] Collaboration (multiple users)
- [ ] Cloud sync (optional)

---

## Competitor Analysis

### GitHub Copilot Workspace / Agent
- ✅ Real Git history, line-by-line diffs, PR review flow
- ✅ Multi-file context, AI proposes changes
- ❌ Developer-centric UX (branches, PRs, CLI)
- ❌ Requires GitHub ecosystem

### GitBook Agent
- ✅ Non-technical-friendly, conversational editing
- ✅ AI drafts, human approves
- ❌ Platform-locked (GitBook only)
- ❌ Docs-focused, not general-purpose

### Git-backed CMS (Netlify CMS, Forestry, etc.)
- ✅ Good Git abstraction for non-technical users
- ❌ AI is "sidecar" — not native to the editing flow
- ❌ Content-focused, not document collaboration

### Notion / Confluence + AI
- ✅ Rich editing, AI suggestions
- ❌ No true version control (page history ≠ Git)
- ❌ No redline review before accepting AI changes

---

## Technical Considerations

### File Format Strategy
1. **Native:** Markdown, plain text, JSON, YAML
2. **Convert on import:** Word → Markdown (pandoc)
3. **Read-only:** PDF (text extraction)
4. **Future:** Native DOCX editing with diff

### Git Abstraction
- User never sees: branches, remotes, merge conflicts, CLI
- User sees: "Save" (= commit), "History" (= log), "Undo" (= revert)
- Under the hood: linear history on main, auto-commit on approve

### AI Integration
- LLM: Claude / GPT-4 via API
- Context window: Full workspace (chunked if large)
- Edit format: Structured diffs (not free-form text replacement)

---

## Origin Story

This concept emerged from a real workflow: using VS Code + GitHub Copilot + Git to write functional specifications for an enterprise software project.

**Key insight:** The productivity came from:
1. **Iterative editing** — AI makes small changes, human reviews, repeat
2. **Redline review** — Without seeing what changed, you can't trust the AI
3. **Workspace context** — AI could reference other documents in the project
4. **Version history** — "What did we change last session?" is always answerable

This workflow is essentially **"Docs-as-Code with AI"** — but it requires technical Git knowledge. The product opportunity is to package this for non-technical users.

---

## Session Kickoff Prompt

When starting a new AI session on this project, use this prompt:

```
I'm working on Redline — a non-technical Git client where humans and AI 
collaborate on document projects with bi-directional redline review.

Key concept: AI edits files in place, human reviews redline before accepting. 
Human edits files, AI can review their redline for issues. Full Git history 
but no CLI/branches exposed to user.

Target users: Lawyers, BAs, compliance officers, technical writers.

Please review this README and help me with [specific task].
```

---

## Project Structure

```
Redline/
├── README.md           # This file
├── docs/
│   ├── architecture.md # Technical design
│   ├── use-cases/      # Detailed use case docs
│   └── competitors.md  # Deep competitor analysis
├── src/                # Source code
├── tests/              # Test files
└── .github/
    └── instructions/   # AI coding guidelines
```

---

*Last updated: January 7, 2026*
