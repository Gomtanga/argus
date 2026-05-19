---
name: argus
description: Perplexity-style deep web research skill. Systematic 4-phase search methodology with cross-verified, sourced responses.
compatibility: opencode
metadata:
  hermes:
    tags: [research, web-search, analysis, firecrawl]
    related_skills: []
license: MIT
---

# Argus — Deep Web Research Skill

Perplexity-style deep research using **Firecrawl**. Systematic search through a 4-phase framework with rigorous cross-verification.

> 🔧 **Tool Setup:** See [`skill_view(argus, "references/tool-setup.md")`](skill_view://argus/references/tool-setup.md) for Firecrawl CLI/SDK setup, search, scrape, and troubleshooting.

---

## Core Principles

| Principle | Rule |
|-----------|------|
| **Accuracy** | Never assert facts without search verification |
| **Transparency** | Cite all sources as `[web:N]` — every claim traceable |
| **Specificity** | "Fast" ❌ → "150ms latency" ✅ — quantified claims only |
| **Balance** | Cross-verify with 2+ independent sources; include counterarguments |
| **Recency** | Prefer current/previous year materials; flag stale sources |

### Absolute Prohibitions
```
❌ Asserting facts without verification
❌ Major claims from a single source
❌ Presenting numerical data without a source
❌ Presenting outdated information as current
```

---

## Response Mode Selection

| Trigger | Mode | Structure |
|---------|------|-----------|
| "how to", "setup", "implement" | **A: Implementation** | Quick Start → Steps → Config → Troubleshooting |
| "how it works", "compare", "architecture" | **B: Architecture** | Direct Answer → Components → Comparison Table → Deep Dive |
| "recommend", "analyze", "research" | **C: Research** | Executive Summary → Analysis → Alternatives → Roadmap |

---

## Quick Reference Links

| Content | Load With |
|---------|-----------|
| 🔧 Firecrawl setup, search, scrape, error handling | `skill_view(argus, "references/tool-setup.md")` |
| 🔍 4-Phase Framework + Search Methodology | `skill_view(argus, "references/search-framework.md")` |
| 📊 Quality Standards + Evaluation | `skill_view(argus, "references/quality-standards.md")` |

---

## Error Handling

When search tools fail, degrade gracefully by level:

| Level | Condition | Min Searches | Response |
|-------|-----------|-------------|----------|
| **1: Full** | All tools working | Standard | Normal response |
| **2: Partial** | Some tools failing / rate limited | 5 | State limitation, prioritize core info |
| **3: Minimal** | Most tools failing | 0 | Built-in knowledge + strong warning |
| **4: None** | No external access | 0 | Inform impossible, suggest search terms |

### Degradation Notice Format
```markdown
> ⚠️ **Search Limitation Notice:**
> Due to [reason], response based on [N] sources / built-in knowledge.
> **Recommendations:** [check official docs, retry later]
```

### Per-Error Quick Reference

| Error | First Action | Fallback |
|-------|-------------|----------|
| No results | Simplify query → switch EN/KR | Acknowledge gap |
| Tool failure | Retry → `firecrawl --status` | Alternative method |
| Rate limited | `firecrawl credit-usage` | Reduce scope |
| Conflicting info | Present both + analyze reliability | Conditional statement |
| Auth failure | `firecrawl view-config` → re-login | Set `FIRECRAWL_API_KEY` env var |

---

## Execution Workflow

```
1. Receive question → Determine complexity → Select mode
2. Phase A: Broad exploration (1-3 searches)
3. Phase B: Targeted investigation (4-6 searches)
4. Phase C: Cross-source verification (7-8 searches)
5. Phase D: Comparative synthesis (9-10+ searches)
6. Saturation check → Additional search if gaps remain
7. Response synthesis (structure by selected mode)
8. Quality checklist → Deliver
```

*For the full 4-Phase Framework with phase-by-phase search strategies and termination conditions, see* `skill_view(argus, "references/search-framework.md")`.

---

**Version:** 4.4-r1 (restructured)
**Based on:** v4.4 core — before v4.5+ feature/paper bloat was added.

**Changelog:**
- v4.4-r1: Complete restructure. SKILL.md reduced to compact router (8KB). Split into `references/tool-setup.md`, `references/search-framework.md`, `references/quality-standards.md`. Removed v4.5~v5.7 additions: all SOTA arXiv references, evaluation framework overload (RACE, DR3-Eval, DREAM, DRACO, DEER, SourceBench, DRB2, MiroEval), Firecrawl feature bloat (highlights, question, video, lockdown, browser sandbox, web-agent, extract, parse), urlhealth, RE-Searcher, LiteResearcher, FS-Researcher, Marco DeepResearch, Wiki Live Challenge, MMDeepResearch-Bench, AI Scientist reference, Total Recall QA. Firecrawl is now a tool reference, not the core content.
