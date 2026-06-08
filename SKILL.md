---
name: argus
description: Perplexity-style deep web research skill. Systematic 4-phase search methodology with evidence-calibrated, sourced responses.
compatibility: opencode
metadata:
  hermes:
    tags: [research, web-search, analysis, firecrawl]
    related_skills: []
license: MIT
---

# Argus — Deep Web Research Skill

Perplexity-style deep research using **Firecrawl**. Systematic search through a 4-phase framework with rigorous cross-verification and conservative evidence calibration.

> 🔧 **Tool Setup:** See [`skill_view(argus, "references/tool-setup.md")`](skill_view://argus/references/tool-setup.md) for Firecrawl CLI/SDK setup, search, scrape, and troubleshooting.

---

## Core Principles

| Principle | Rule |
|-----------|------|
| **Accuracy** | Never assert facts without search verification |
| **Transparency** | Cite all sources as `[web:N]` — every claim traceable |
| **Specificity** | Use numbers and metrics only when directly relevant and source-verifiable |
| **Balance** | Cross-verify with independent sources; include counterarguments and uncertainty |
| **Recency** | Prefer current/previous year materials; flag stale sources |

### Evidence Calibration Rules

Before final synthesis, classify each major claim:

| Level | Use When |
|-------|----------|
| **Verified** | Confirmed by an official/primary source for the exact claim |
| **Supported** | Backed by multiple credible secondary sources, but no primary confirmation |
| **Anecdotal** | Based mainly on community posts, forums, Reddit, or personal blogs |
| **Unverified** | Found in only one weak source or not directly confirmed |
| **Speculative** | Reasoned inference from available evidence |

Do not upgrade a claim's confidence level just because multiple low-quality sources repeat it.

### Strong Language Guardrail

Use strong terms only when the evidence qualifies:

| Term | Required Evidence |
|------|-------------------|
| "standard", "industry standard", "widely adopted" | Official standards, adoption data, or multiple independent primary/enterprise sources |
| "production-proven", "verified in production" | Case studies, official deployment reports, or reproducible production evidence |
| "community consensus" | Broad, independent community evidence across multiple platforms |
| "verified" | Direct evidence for the exact claim, preferably from primary sources |

Otherwise use softer language: "appears to be", "is supported by", "is reported by", "likely", "suggests", "requires verification", or "not enough evidence to conclude".

### Absolute Prohibitions
```
❌ Asserting facts without verification
❌ Major claims from a single source
❌ Presenting numerical data without a source
❌ Forcing numbers to satisfy a quota
❌ Treating repeated low-quality sources as independent verification
❌ Presenting outdated information as current
❌ Calling something "standard", "production-proven", or "verified" without qualifying evidence
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
| 🐍 Firecrawl Python SDK quirks & pitfalls | `skill_view(argus, "references/firecrawl-python-sdk-quirks.md")` |
| 🔍 4-Phase Framework + Search Methodology | `skill_view(argus, "references/search-framework.md")` |
| 📊 Quality Standards + Evidence Calibration | `skill_view(argus, "references/quality-standards.md")` |

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
6. Evidence calibration → Classify major claims and downgrade weak evidence
7. Gap check → Additional search if primary-source coverage is missing
8. Response synthesis (structure by selected mode)
9. Quality checklist → Deliver
```

*For the full 4-Phase Framework with phase-by-phase search strategies and termination conditions, see* `skill_view(argus, "references/search-framework.md")`.

---

**Version:** 4.5 (Evidence Calibration Edition)
**Based on:** v4.4-r1 compact core with stricter evidence judgment.

**Changelog:**
- v4.5: Evidence Calibration Edition. Added claim confidence levels, strong-language guardrails, numeric claim rules, source hierarchy requirements, and Claim Coverage Matrix guidance. Removed the implied quantified-metric quota in favor of directly relevant, source-verifiable numbers.
- v4.4-r1: Complete restructure. SKILL.md reduced to compact router (8KB). Split into `references/tool-setup.md`, `references/search-framework.md`, `references/quality-standards.md`. Removed earlier experimental feature/paper bloat and kept Firecrawl as a tool reference, not the core content.
