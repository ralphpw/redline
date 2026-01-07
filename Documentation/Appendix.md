# Appendix: Discovery Process

This document captures the key insights and decision points from the initial product discovery session that led to Redline.

---

## Origin: The "Aha" Moment

While using VS Code + GitHub Copilot + Git to write functional specifications for an enterprise software project (Asset Registry), the user observed:

> "The specification process that we are following: iteration/text/git/diff-based, VS Code/agent-based — I'm finding it AMAZINGLY productive for all kinds of use cases OUTSIDE of coding."

This led to the question: **"Is this a well-documented approach?"**

The answer: No. It's an emerging pattern at the intersection of:
- **Docs-as-Code** (treating documentation like source code)
- **Git-based collaboration** (version control for non-code)
- **AI agents** (LLMs that edit files in place)

No one had packaged this for non-technical users.

---

## The Core Insight

When asked why existing tools (Notion, Google Docs, ChatGPT) don't solve this:

> **"Without version control you can't see what that last change was, which makes this workflow impossible."**

This is the fundamental gap:
- AI tools edit text, but don't show diffs
- Collaboration tools have history, but not atomic, reviewable commits
- Version control exists, but requires technical knowledge

---

## The Bi-Directional Differentiator

User's emphasis on what makes this unique:

> **"Key is the complexity enabled through bi-directional reviews."**

This means:
1. **Human → AI review:** AI makes changes, human reviews diff before accepting
2. **AI → Human review:** Human makes changes, AI reviews diff for issues

This bi-directional trust loop is what enables complex, multi-session projects. Neither party works blindly.

---

## Market Validation (Perplexity Research)

Queried for existing products. Results confirmed:

| Product | Gap |
|---------|-----|
| **GitHub Copilot Agent** | Real Git + diff, but developer-centric |
| **GitBook Agent** | Non-technical friendly, but platform-locked |
| **Git-backed CMS** | Good Git abstraction, but AI is "sidecar" |
| **Notion/Confluence** | No true version control, no diff review |

**Conclusion:** Market gap exists. No general-purpose, non-technical tool with Git + AI-native editing + diff review.

---

## Why "Redline"

The name emerged from a list of candidates. Final selection rationale:

| Criterion | Why Redline Works |
|-----------|-------------------|
| **Instant recognition** | Lawyers, editors, anyone who's reviewed documents knows "redline" |
| **Implies core mechanic** | Track changes, diff, markup — the product's central feature |
| **Professional tone** | Enterprise-ready; not cutesy or overly "AI-forward" |
| **Short and memorable** | One word, easy domain, easy to say |
| **Legal market entry** | Strong positioning for initial target user (legal teams) |

Other considered names:
- **Duet** — Clean, but less descriptive
- **Trustline** — Good, but "Redline" is more direct
- **GlideAI** — Emphasizes UX, but buries the core feature
- **Corevise** — Accurate, but harder to remember

---

## The Email Workflow Insight

User identified email drafting as a broken workflow that Redline solves:

**Current state:**
1. Draft email
2. Paste into ChatGPT
3. AI rewrites (what changed? no idea)
4. Paste back
5. No history, no diff, no context

**With Redline:**
- Email project folder with context files
- AI edits in place
- Diff shows exactly what changed ("Changed 'I think' → 'I recommend'")
- Full history for iteration

This use case may be the wedge for broader adoption — everyone writes important emails.

---

## Technical Direction

Initial decision: **Start with VS Code extension**

Rationale:
1. Fastest path to MVP (leverage existing editor, Git, diff views)
2. Validates workflow before building custom UI
3. Technical users can adopt immediately
4. Learnings inform standalone app later

Migration path: VS Code Extension → Electron App (for non-technical users)

---

## Key Quotes from Discovery

> "I'll then turn this into a GitHub project we can work on together, honestly it sounds killer."

> "Key is the complexity enabled through bi-directional reviews."

> "Without version control you can't see what that last change was, which makes this workflow impossible."

> "The productivity came from iterative editing — AI makes small changes, human reviews, repeat."

---

## Session Context

- **Date:** January 7, 2026
- **Starting point:** Asset Registry requirements specification work
- **Pivot:** Recognized methodology as product opportunity
- **Output:** Full product spec, competitive analysis, architecture options
- **Repo:** Redline initialized with first commit

---

*This appendix preserves the discovery process for future reference. The README.md contains the synthesized product vision.*
