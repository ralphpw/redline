# Use Case: Legal Document Review

## Scenario

A legal team is preparing a contract suite for a major acquisition:
- Master Purchase Agreement (MPA)
- 5 Schedules (A-E)
- 3 Exhibits
- Side letter

**Total:** 9 interconnected documents, ~200 pages

---

## Current Workflow (Pain Points)

1. **Drafting** — Lawyer drafts in Word, sends to partner
2. **Review** — Partner redlines in Track Changes, sends back
3. **Iteration** — Multiple rounds of redlines, versions proliferate
4. **AI assist** — Lawyer pastes clause into ChatGPT, gets suggestions, pastes back
   - ❌ No diff — what did AI change?
   - ❌ No context — AI doesn't see the rest of the contract
5. **Cross-reference hell** — "Update the indemnity cap everywhere" = manual search
6. **Version chaos** — `MPA_v3_final_FINAL_reviewed_v2.docx`

---

## With Redline

### Setup
```
AcquisitionDeal/
├── MPA.md                 # Master Purchase Agreement
├── Schedule-A.md          # Definitions
├── Schedule-B.md          # Assets
├── Schedule-C.md          # Liabilities
├── Schedule-D.md          # Employees
├── Schedule-E.md          # IP
├── Exhibit-1.md           # Form of Bill of Sale
├── Exhibit-2.md           # Form of Assignment
├── Exhibit-3.md           # Form of Opinion
└── Side-Letter.md         # Ancillary agreements
```

### Workflow

**1. Cross-doc edit:**
> "Update the indemnity cap from $5M to $7.5M everywhere it appears"

AI finds all 6 occurrences across 4 documents, shows unified redline:

```diff
# MPA.md
- subject to an aggregate cap of Five Million Dollars ($5,000,000)
+ subject to an aggregate cap of Seven Million Five Hundred Thousand Dollars ($7,500,000)

# Schedule-C.md  
- Indemnification Cap: $5,000,000
+ Indemnification Cap: $7,500,000

# Side-Letter.md
- notwithstanding the $5M cap in the MPA
+ notwithstanding the $7.5M cap in the MPA
```

**2. Consistency check:**
> "Review my changes — are there any inconsistencies?"

AI scans all documents, reports:
- ⚠️ Schedule-A defines "Cap" as $5M but MPA now says $7.5M
- ✅ All date references consistent
- ⚠️ Exhibit-2 still references old cap in recitals

**3. Clause generation with context:**
> "Add a force majeure clause that's consistent with our liability limitations"

AI reads existing limitation of liability section, generates clause that references the same defined terms, same cap structure.

**4. Partner review:**
Partner opens Redline, sees history:
- Tuesday: Initial draft
- Wednesday: AI-assisted indemnity cap update
- Thursday: Manual edits to representations

Clicks each commit to see exactly what changed.

---

## Key Benefits for Legal

| Pain Point | Redline Solution |
|------------|------------------|
| Cross-doc consistency | AI sees all files, finds all occurrences |
| AI trust | Redline shows exactly what AI changed |
| Version control | Git history, not filename suffixes |
| Audit trail | Every change attributed + timestamped |
| Collaboration | Review partner's changes via diff |

---

## Compliance Angle

For regulated industries (finance, healthcare):
- **Immutable history** — Git commits can't be silently edited
- **Attribution** — Every change has author
- **Rollback** — "Show me the version we sent to the board"
- **Export** — Generate redline PDF for external parties

---

*Last updated: January 7, 2026*
