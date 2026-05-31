# 🔍 4-Phase Search Framework + Methodology

The core of Argus. A systematic, non-linear research process that adapts per query type.

---

## Phase A: Broad Exploration (1-3 searches)

Build initial understanding — identify key concepts, players, and landscape.

```
A.1 Core Concepts: "[Topic] what is how it works fundamentals"
A.2 Current State: "[Topic] 2025 2026 current state adoption"
A.3 Architecture/Overview: "[Topic] architecture components overview"

Checkpoint:
□ 3-5 core concepts identified
□ Key statistics secured
□ Gaps identified for Phase B investigation
```

> 💡 *If the topic is entirely unfamiliar, do all 3. For familiar topics, skip to Phase B.*

---

## Phase B: Targeted Investigation (4-6 searches)

Dive deep — gather implementation details, performance data, real-world problems.

```
B.1 Implementation: "[Topic] tutorial implementation example guide"
B.2 Performance: "[Topic] performance benchmark metrics comparison"
B.3 Problems: "[Topic] common problems issues limitations pitfalls"

Checkpoint:
□ 5+ quantified metrics
□ 3+ common problems documented
□ Performance characteristics understood
□ Enough depth to verify claims in Phase C
```

---

## Phase C: Cross-Source Verification (7-8 searches)

The most critical phase — verify everything. Multi-source cross-checking.

```
C.1 Multi-Source Check: "[Topic] verify claims across sources"
C.2 Criticism: "[Topic] review analysis pros cons limitations"
C.3 Community Reality: "[Topic] Reddit experience real user feedback"

Cross-source verification rules:
□ Every major claim verified with 2+ independent sources
□ Official docs prioritized over tutorials/secondary sources
□ Conflicting information explicitly noted (don't hide it!)
□ Community sentiment checked — real users vs marketing
□ Stale content flagged by date
```

> 💡 **Quality heuristic:** If a fact comes from a single source with no corroboration, flag it as "unverified."
> 💡 **Recency check:** Prefer 2025-2026 materials. If only older sources exist, state the date limitation.

---

## Phase D: Comparative Synthesis (9-10+ searches)

Compare alternatives, identify gaps, look forward.

```
D.1 Comparison: "[Topic] vs [Alt1] vs [Alt2] comparison differences"
D.2 Outlook: "[Topic] future roadmap 2025 2026 trends"
D.3 Gap Fill: Additional searches for unresolved questions

Checkpoint:
□ 3+ alternatives compared
□ Future outlook understood
□ Information saturation — last 2 searches yielded nothing new
```

> ⚠️ **Phase D는 복잡도 4-5일 때만 진행** — simpler queries should terminate after Phase C.

---

## Search Termination Conditions

### STOP when ALL are met:
```
✅ Clear answers to all core questions
✅ Major claims verified from 2+ independent sources
✅ Counterarguments and limitations documented
✅ No new information in last 2 consecutive searches
✅ Minimum search count met (by complexity)
```

### CONTINUE when ANY is true:
```
❌ Core numerical data missing
❌ Only one perspective collected
❌ Latest (2025-2026) information not secured
❌ Obvious information gaps remain
```

---

## Search Guide by Complexity

| Complexity | Min Searches | Example |
|-----------|-------------|---------|
| 1-2 (Simple) | 1-3 | "What is Docker?", "Python version" |
| 3 (Moderate) | 3-6 | "How to use React hooks", "AWS EC2 setup" |
| 4 (Significant) | 5-10 | "React vs Vue comparison", "K8s architecture" |
| 5 (Deep) | 8-14 | "Production migration strategy" |

## Query Optimization

- **Specificity Level 2-3**: `"React 18 performance optimization"` ← Goldilocks zone
- **site: operator**: `site:docs.docker.com networking`
- **Recency tagging**: Include current/previous year in query
- **Language switch**: Search same topic in EN and KR for broader coverage
- **Time filters**: Use `--tbs qdr:y` (year) / `qdr:m` (month) / `qdr:w` (week)
- **Pitfall avoidance**: "tutorial" → best for Phase B; "review" → best for Phase C

> 💡 *For technical topics, add "2025" or "latest" to the query. For well-established topics, remove year for historical coverage.*
