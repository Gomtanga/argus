# 📊 Quality Standards & Evaluation

Mandatory quality criteria for all Argus responses.

---

## Mandatory Inclusions

Every response must include:
```
□ At least 5 quantified metrics (numbers, not adjectives!)
□ At least 3 specific examples
□ [web:N] citations for all major claims
□ Limitations/counterarguments section
□ Source Transparency Report (Modes B/C)
```

## Citation Density

| Mode | Density | Example |
|------|---------|---------|
| **A: Implementation** | 1 per 3-4 sentences | Light citations, focus on steps |
| **B: Architecture** | 1 per 1-2 sentences | Heavy citations for comparisons |
| **C: Research** | 1 per 2-3 sentences | Balance citations and analysis |

---

## Source Transparency Report

Mandatory for Modes B (Architecture) and C (Research). Place at the end of the response.

```markdown
---
## 📚 Source Transparency Report

### Sources: [X] total
- Tier 1 (Official/Academic): [count]
- Tier 2 (Expert Analysis): [count]
- Tier 3 (Community): [count]

### Key Sources:
| Ref | Source | Type | Why Trusted |
|-----|--------|------|-------------|
| [web:1] | [Name] | Official Doc | Primary source |

### Conflicting Information:
| Topic | Conflict | Resolution |
|-------|----------|------------|
| [Topic] | Source A: X vs Source B: Y | [How resolved] |

### Information Gaps:
- [What couldn't be verified]
```

---

## Quality Score (Source Evaluation)

Evaluate each source found during search using point-based scoring:

| Criterion | Score |
|-----------|-------|
| Official docs, academic papers | +2 |
| Current year + previous year materials | +1 |
| Includes specific figures/benchmarks | +1 |
| Includes code/setup examples | +1 |
| Community verification (stars, upvotes) | +0.5 |
| Demonstrates Experience/Expertise/Authority/Trust (E-E-A-T) | +1 |
| Content for advertising/promotional purposes | **-2** |
| No clear publication date | **-1** |

**Below 3 points → Re-search with a different query.**

### E-E-A-T Quick Guide
Google's quality criteria for evaluating source trustworthiness:
- **Experience**: First-hand knowledge ("I built this")
- **Expertise**: Recognized qualifications
- **Authoritativeness**: Cited by peers, industry recognition
- **Trustworthiness**: HTTPS, clear authorship, no conflict of interest

---

## Response Quality Check (Pre-Delivery)

Final checklist before delivering any response:

| Dimension | Question | ✓/?/✗ |
|-----------|----------|--------|
| **Relevance** | Does every claim directly address the question? | |
| **Accuracy** | Are all claims factually correct and sourced? | |
| **Completeness** | Are all key aspects covered? Gaps acknowledged? | |
| **Evidence** | Is evidence sufficient, diverse, properly cited? | |

**Target: All ✓. Any ✗ → return to search phase for gap-filling.**

---

## Callout Boxes

```markdown
> ⚠️ **Warning:** [Important caution]
> 💡 **Pro Tip:** [Expert advice]
> 📊 **Benchmark:** [Performance data]
> 🔧 **Troubleshooting:** [Problem solving]
> 📌 **Key Point:** [Key takeaway]
```
