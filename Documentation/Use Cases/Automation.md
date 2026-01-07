# Use Case: AI-Powered Book Production

## Scenario

A creator is producing a book (translation, adaptation, or original work) using AI agents for various production tasks. The project involves:

- 22 chapters to process
- 140+ individual AI tasks (tickets)
- Multiple AI models for different jobs
- Multi-phase pipeline (research → writing → review → publication)
- 6-9 month timeline

**This is NOT just collaboration. This is orchestrated automation with human oversight.**

---

## The Hybrid Workflow

Collaboration and Automation aren't separate products — they're phases:

```
┌─────────────┐    ┌─────────────┐    ┌─────────────┐    ┌─────────────┐
│ COLLABORATE │───►│ TEMPLATIZE  │───►│  AUTOMATE   │───►│   REVIEW    │
│             │    │             │    │             │    │             │
│ Develop the │    │ Save as     │    │ Run against │    │ AI reviews  │
│ process     │    │ reusable    │    │ all inputs  │    │ the outputs │
│ together    │    │ ticket      │    │ overnight   │    │ for you     │
└─────────────┘    └─────────────┘    └─────────────┘    └─────────────┘
      ▲                                                        │
      └────────────────────────────────────────────────────────┘
                         Iterate on process
```

### Example: Legal Transcript Summarization

1. **Collaborate:** Lawyer works with AI on 3 sample deposition transcripts
   - "Make the summary more focused on admissions"
   - "Include timestamps for key statements"
   - "Flag inconsistencies with prior testimony"
   
2. **Templatize:** Save the refined process as a ticket
   ```
   Ticket: Deposition Summary
   Inputs: transcript.md, prior-testimony.md
   Process: [the approach you developed together]
   Output: summary.md with flagged inconsistencies
   ```

3. **Automate:** Execute against 47 remaining transcripts
   - Runs overnight
   - Logs: model, tokens, cost per transcript
   - Outputs: 47 summary files

4. **Review:** AI reviews the batch
   - "Which summaries have the most flagged inconsistencies?"
   - "Show me the redline between transcript and summary for #23"
   - "Any summaries that seem unusually short?"

**The lawyer reviews the AI's review, not 47 transcripts.**

---

## Two Modes of Redline

| Mode | Collaboration | Automation |
|------|---------------|------------|
| **Metaphor** | Pair programming | Assembly line |
| **Human role** | Co-author | Supervisor/QA |
| **AI role** | Partner | Worker |
| **Interaction** | Conversational | Ticket-driven |
| **Review** | Every edit | Batch/milestone |
| **Use cases** | Email, contracts, specs | Books, datasets, pipelines |

**Collaboration:** "AI, help me write this paragraph better"  
**Automation:** "AI, execute ticket T018: Translate Chapter 1 using these inputs"

---

## Real Example: Blindsight Translation Project

A translation of Peter Watts' *Blindsight* using AI agents with full auditability:

### Project Structure
```
Blindsight/
├── README.md                    # Project context for AI onboarding
├── Documentation/               # Human-readable project docs
│   ├── 1.1-Project-Overview.md
│   ├── 2.1-Character-Philosophy.md
│   └── 4.1-Technical-Approach.md
├── Instructions/                # LLM-executable tickets
│   ├── Tickets/
│   │   ├── T001-Acquire-Source.md
│   │   ├── T003-Keeton-Character-Study.md
│   │   ├── T018-Translate-Chapter-01.md
│   │   ├── T200-Illustration-Master-Orchestrator.md
│   │   └── [140+ tickets...]
│   └── Guidelines/
│       ├── Character-Voice-Reference.md
│       └── Illustration-Generation-Guide.md
├── Tasks/
│   ├── KANBAN.md               # Source of truth for all progress
│   ├── 01-Research/
│   ├── 02-Summaries/
│   ├── 03-Translation/
│   ├── 04-Reviews/
│   └── 05-Publication/
└── Output/
    └── Illustrations/
```

### What a Ticket Looks Like

```markdown
# T018: Translate Chapter 01
**Stage:** Translation
**LLM:** Opus 4.1
**Token Budget:** 200K limit / 55K required
**Estimated Cost:** $15
**Dependencies:** T001, T003-T007, T013, T014, T015

---

## Context Requirements
- Source chapter: `Tasks/01-Research/source/chapter-01.md`
- Character studies: T003-T007 outputs
- Jargon dictionary: T013 output
- Chapter summary: T014 output

## Task Instructions
1. Load all context files
2. Apply translation rules from Character-Voice-Reference.md
3. Maintain voice consistency per character
4. Output to `Tasks/03-Translation/chapter-01-translated.md`

## Quality Gates
- [ ] All jargon terms handled per dictionary
- [ ] Character voices match reference profiles
- [ ] No anachronistic language introduced
```

---

## The Automation Value Proposition

### Without Redline (Current State)

```
┌─────────────┐    ┌─────────────┐    ┌─────────────┐
│ Human runs  │───►│ AI produces │───►│ Human saves │
│ prompt      │    │ output      │    │ somewhere   │
└─────────────┘    └─────────────┘    └─────────────┘
                          │
                    No audit trail
                    No context for next task
                    No diff of what AI did
                    Manual progress tracking
```

**Problems:**
- **No provenance** — "Which model produced this? What prompt?"
- **No auditability** — "What exactly did the AI change?"
- **Manual orchestration** — Human tracks 140 tasks in spreadsheet/head
- **Context loss** — Each AI session starts fresh
- **No rollback** — Bad output = start over

### With Redline (Automation Mode)

```
┌─────────────────────────────────────────────────────────┐
│                    KANBAN VIEW                          │
│  ┌─────────┐ ┌─────────┐ ┌─────────┐ ┌─────────┐       │
│  │ TODO    │ │ RUNNING │ │ REVIEW  │ │ DONE    │       │
│  │ T019    │ │ T018    │ │ T003    │ │ T001    │       │
│  │ T020    │ │         │ │ T004    │ │ T002    │       │
│  │ ...     │ │         │ │         │ │ ...     │       │
│  └─────────┘ └─────────┘ └─────────┘ └─────────┘       │
└─────────────────────────────────────────────────────────┘
                          │
                    Click to execute
                          │
                          ▼
┌─────────────────────────────────────────────────────────┐
│ EXECUTING: T018 - Translate Chapter 01                  │
│                                                         │
│ Model: Opus 4.1                                         │
│ Context loaded: 55K tokens (5 files)                    │
│ Status: Running...                                      │
│                                                         │
│ [=============================>          ] 73%          │
└─────────────────────────────────────────────────────────┘
                          │
                    Execution complete
                          │
                          ▼
┌─────────────────────────────────────────────────────────┐
│ REVIEW: T018 Output                                     │
│                                                         │
│ Output: chapter-01-translated.md (12,847 words)         │
│                                                         │
│ REDLINE VIEW:                                           │
│ + 12,847 lines added                                    │
│                                                         │
│ EXECUTION LOG:                                          │
│ - Model: claude-3-opus-20240229                         │
│ - Tokens: 54,892 input / 18,234 output                  │
│ - Cost: $14.23                                          │
│ - Duration: 4m 32s                                      │
│                                                         │
│ [APPROVE & COMMIT]    [REJECT & RETRY]    [EDIT]       │
└─────────────────────────────────────────────────────────┘
```

---

## Key Automation Features

| Feature | Why It Matters |
|---------|----------------|
| **Ticket-driven execution** | Each task is atomic, reproducible, auditable |
| **Dependency tracking** | T018 can't run until T001-T015 complete |
| **Execution logs** | Model, tokens, cost, duration — all recorded |
| **Batch review** | Review 10 completed tickets at once |
| **Rollback per ticket** | Bad T018? Revert just that task |
| **Cost tracking** | "$623 spent, $67 remaining in budget" |
| **Progress dashboard** | "47/140 tickets complete (34%)" |

---

## Who Uses Automation Mode?

1. **Authors/Translators** — Book production pipelines
2. **Dataset creators** — Labeling, annotation, transformation at scale
3. **Content teams** — Bulk content generation with QA
4. **Researchers** — Literature review, systematic extraction
5. **Agencies** — Multi-deliverable projects with auditable AI use

---

## The Meta-Irony

The Blindsight example is particularly fitting:

> "We're using the very technology Watts predicted (LLMs) to make his prescient work accessible, creating a meta-ironic commentary where a Chinese Room translates a novel about Chinese Rooms."

The AI agents don't understand the book. They execute tickets. They're "Chinese Rooms" — symbol manipulators without comprehension. But the output is auditable, reviewable, and version-controlled.

**That's the power of Redline in automation mode: you don't need to trust the AI. You verify.**

---

## Comparison: Collaboration vs Automation

| Dimension | Collaboration Mode | Automation Mode |
|-----------|-------------------|-----------------|
| **Session length** | Long, conversational | Short, task-focused |
| **AI context** | Full workspace | Ticket-specified files |
| **Human attention** | Every edit | Milestone review |
| **Diff granularity** | Line-by-line | File/ticket level |
| **Typical project** | 1-10 documents | 50-500 documents |
| **Timeline** | Hours to days | Weeks to months |
| **Cost model** | Per-conversation | Per-ticket budgets |

**Both modes share:**
- Git-based version control
- Redline review before commit
- Full audit trail
- Rollback capability

---

*Last updated: January 7, 2026*
