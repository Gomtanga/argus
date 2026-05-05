---
name: argus
description: Perplexity-style deep web research skill for comprehensive, verified information gathering. Use when user needs web search, comparison, analysis, or investigation.
compatibility: opencode
metadata:
  hermes:
    tags: [research, web-search, analysis, firecrawl]
    related_skills: []
license: MIT
---

# Argus - Deep Web Research Skill

A systematic web search skill in the style of Perplexity. Uses **Firecrawl CLI** (or Python SDK as fallback) to conduct deep research through a 4-phase framework with memory-guided agentic workflows. Supports **Hermes Agent** and **OpenCode** environments.

---

## Tool Setup

Argus uses Firecrawl for web search. Two ways to access it:

### Method 1: Firecrawl CLI (Recommended)

Install the Firecrawl CLI tool (package renamed in v2 — use `firecrawl-cli`):

```bash
# New package name (v2+)
npm install -g firecrawl-cli

# Or use npx for one-off use
npx -y firecrawl-cli@latest init --all --browser
```

Authenticate with your API key:

```bash
# Interactive login (opens browser or prompts for API key)
firecrawl login

# Login with API key directly
firecrawl login --api-key fc-YOUR-API-KEY

# Or set via environment variable
export FIRECRAWL_API_KEY="your-api-key"
export FIRECRAWL_BASE_URL="https://firecrawl.jiminbox.com"

# Verify installation and authentication
firecrawl --status
```

#### Available CLI Commands (v1.16+)

| Command | Purpose | Example |
|---------|---------|---------|
| `firecrawl search <query>` | Web search | `firecrawl search "Python 3.13 features"` |
| `firecrawl scrape <url>` | Extract page content | `firecrawl scrape https://example.com` |
| `firecrawl crawl <url>` | Crawl a website | `firecrawl crawl https://docs.example.com` |
| `firecrawl map <url>` | Map site structure | `firecrawl map https://docs.example.com` |
| `firecrawl interact <prompt>` | Interact with scraped page (click, form) | `firecrawl interact "Search for iPhone price"` |
| `firecrawl agent <prompt>` | AI agent for structured research | `firecrawl agent "Top 5 AI startups funding" --wait` |
| `firecrawl credit-usage` | Check remaining credits | `firecrawl credit-usage --json` |
| `firecrawl login` | Authenticate with API key | `firecrawl login --api-key fc-key` |
| `firecrawl view-config` | View current configuration | `firecrawl view-config` |
| `firecrawl --status` | Check version, auth, concurrency | `firecrawl --status` |

#### Advanced CLI Options

```bash
# Search with time filters (hour/day/week/month/year)
firecrawl search "deep learning" --tbs qdr:y

# Search and auto-scrape results
firecrawl search "AI news" --limit 10 --scrape

# Filter by source and category
firecrawl search "react hooks" --sources web,news --categories github

# Scrape with clean output (remove nav/footer/ads)
firecrawl scrape https://example.com --only-main-content

# Multiple output formats
firecrawl scrape https://example.com --format markdown,summary,links

# Crawl with advanced options
firecrawl crawl https://example.com --limit 100 --max-depth 3 --wait --progress

# Agent for structured research tasks
firecrawl agent "Compare pricing" --urls site1.com,site2.com --wait
```

Output is Markdown by default — LLM-friendly and easy to consume.

### Method 2: Python SDK (Fallback)

When CLI is not available or you need programmatic control:

```python
from firecrawl import FirecrawlApp

app = FirecrawlApp(
    api_key="your-api-key",
    api_url="https://firecrawl.jiminbox.com"
)

# Search (returns pydantic model — use model_dump())
result = app.search(query="term", limit=5)
data = result.model_dump()
items = data.get("web", [])

# Scrape
page = app.scrape("https://example.com")
content = page.model_dump()
markdown = content.get("markdown", "")

# Map
site_map = app.map(url="https://docs.example.com")
map_data = site_map.model_dump()
```

---

## Query Optimization Tips

- **Level 2-3 Specificity**: `"React 18 performance optimization"` (Appropriate)
- **site: operator**: `site:docs.docker.com networking`
- **Recency**: `"latest 2025 2026 trends updates"`
- **English + Korean**: Switch between EN/KR for broader coverage
- **Time filters**: Use `--tbs qdr:y` (year) / `qdr:m` (month) / `qdr:w` (week) for recency control
- **Category filters**: Use `--categories github,research,pdf` to target source types

---

## Core Principles

| Principle | Description |
|------|------|
| **Accuracy** | Prohibits providing unverified information - must verify through search |
| **Transparency** | Cite sources for all claims in `[web:N]` format |
| **Specificity** | "Fast" ❌ → "150ms latency" ✅ |
| **Balance** | Prohibits reliance on a single source, requires cross-verification with 2+ independent sources |

## Absolute Prohibitions

```
❌ Asserting facts without verification
❌ Making major claims based on a single source
❌ Presenting numerical data without a source
❌ Presenting outdated information as current
```

---

## 4-Phase Search Framework

> 💡 **Modern Deep Research Insight (2026):** Static ReAct-style linear tool chains are being superseded by **memory-guided agentic workflow synthesis** (e.g., FlowSearcher, ICLR 2026). The agent should dynamically plan subgoals, adapt tool ordering per query type, and reuse past workflow patterns via hierarchical memory. The 4-phase framework below encodes this principle as a structured process.

### Phase A: Broad Exploration (1-3 times)
```
A.1 Core Concepts: "[Topic] what is how it works fundamentals"
A.2 Current State: "[Topic] 2025 2026 current state adoption"
A.3 Architecture: "[Topic] architecture components design"

Checkpoint:
□ Identify 3-5 core concepts
□ Secure adoption rates/statistics
□ Identify gaps to investigate in Phase B
```

### Phase B: Targeted Investigation (4-6 times)
```
B.1 Implementation: "[Topic] tutorial implementation example"
B.2 Performance: "[Topic] performance benchmark metrics"
B.3 Problems: "[Topic] common problems issues solutions"

Checkpoint:
□ 5+ specific numbers/metrics
□ 3+ common problems
□ Document performance characteristics and limitations
```

### Phase C: Verification (7-8 times)
```
C.1 Criticism: "[Topic] review analysis pros cons limitations"
C.2 Community: "[Topic] Reddit experience real user feedback"

Checkpoint:
□ Verify major claims with 2+ independent sources
□ Identify and document conflicting information
□ Evaluate community sentiment
```

### Phase D: Comparative (9-10+ times)
```
D.1 Alternatives: "[Topic] vs [Alt1] vs [Alt2] comparison"
D.2 Future: "[Topic] future roadmap 2025 trends"
D.3+ Gap Filling: Supplement remaining unresolved information

Checkpoint:
□ Compare 3+ alternatives
□ Grasp future outlook
□ Confirm information saturation
```

---

## Search Termination Conditions

### Stop (When ALL are met)
```
✅ Secured clear answers to core questions
✅ Verified major claims from 2+ independent sources
✅ Secured information on counterarguments and limitations
✅ No meaningful new information in 2 consecutive searches
✅ Met minimum number of searches (by complexity)
```

### Continue (When ANY are met)
```
❌ Lack of core numerical data
❌ Only one perspective collected
❌ Latest information not secured
❌ Obvious information gaps exist
```

---

## Response Mode Selection

| Trigger | Mode | Structure |
|--------|------|------|
| "how to", "setup", "implement", "method" | **A: Implementation** | Quick Start → Steps → Config → Troubleshooting |
| "how it works", "compare", "architecture", "difference" | **B: Architecture** | Direct Answer → Components → Comparison Table → Deep Dive |
| "recommend", "analyze", "pros cons", "research" | **C: Research** | Executive Summary → Analysis → Alternatives → Roadmap |

---

## Response Quality Standards

### Mandatory Inclusions
```
□ At least 5 quantified metrics (Numbers!)
□ At least 3 specific examples
□ Citing [web:N] for all major claims
□ Inclusion of limitations/counterarguments section
□ Source Transparency Report (Modes B/C)
```

### Citation Density
```
Implementation (A): 1 per 3-4 sentences
Architecture (B): 1 per 1-2 sentences  
Research (C): 1 per 2-3 sentences
```

### Source Transparency Report (Mandatory for Modes B/C)
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
| [Topic] | A: X vs B: Y | [How resolved] |

### Information Gaps:
- [What couldn't be verified]
```

---

## Search Guide by Complexity

| Complexity | Number of Searches | Example |
|--------|-----------|------|
| 1-2 (Simple) | 1-3 times | "What is Docker?", "Python version" |
| 3 (Moderate) | 3-6 times | "How to use React hooks", "AWS EC2 setup" |
| 4 (Significant) | 5-10 times | "React vs Vue performance comparison", "Kubernetes architecture" |
| 5 (Deep) | 8-14 times | "Production Kubernetes migration strategy" |

---

## Quality Score (Search Result Evaluation)

| Criterion | Score |
|------|------|
| Official docs, academic materials | +2 |
| Latest materials from current year and previous year | +1 |
| Includes specific figures, benchmarks | +1 |
| Includes actual code, setup examples | +1 |
| Community verification (upvotes, stars) | +0.5 |
| Demonstrates E-E-A-T (Experience, Expertise, Authority, Trustworthiness) | +1 |
| Content for advertising/promotional purposes | -2 |
| Unclear date | -1 |

> 📌 **E-E-A-T:** Google's Search Quality Rater Guidelines evaluate pages on **Experience, Expertise, Authoritativeness, and Trustworthiness**. First-hand experience (e.g., "I built this"), recognized expertise (e.g., official docs, academic papers), established authority (e.g., cited by peers), and trust signals (e.g., HTTPS, clear authorship, no conflict of interest) all contribute to source quality.

**Below 3 points → Consider re-searching with a different query**

---

## Error Handling

### Graceful Degradation Levels

| Level | Condition | Search Count | Response |
|-------|-----------|-------------|----------|
| **1: Full** | All tools working | Standard protocol | Normal response structure |
| **2: Partial** | Some tools failing / rate limited | Min 5 | State limitation, prioritize core info |
| **3: Minimal** | Most tools failing | 0 | Built-in knowledge + strong warning |
| **4: None** | No external access possible | 0 | Honestly inform impossible, suggest manual search terms |

### Degradation User Notice (Level 2-4)

```markdown
> ⚠️ **Search Limitation Notice:**
> Due to [reason], this response is based on [N] sources / built-in knowledge.
> **Limitations:** [what couldn't be verified]
> **Recommendations:** [check official docs directly, retry shortly, etc.]
```

### Per-Error Response

| Error Type | First Action | Fallback |
|------------|--------------|----------|
| No results | Simplify query → Switch EN/KR | Acknowledge gap |
| Tool failure | Retry once → Run `firecrawl --status` to diagnose | Alternative tool |
| Rate limit | Run `firecrawl credit-usage` to check remaining quota | Reduce scope → Min 5 searches |
| Conflicting info | Present both + analyze reliability | Conditional statement |
| Auth failure | Run `firecrawl view-config` → re-login with `firecrawl login` | Set env var `FIRECRAWL_API_KEY` |

---

## Callout Boxes

```markdown
> ⚠️ **Warning:** [Precautions]
> 💡 **Pro Tip:** [Expert advice]
> 📊 **Benchmark:** [Performance data]
> 🔧 **Troubleshooting:** [Problem solving]
> 📌 **Key Point:** [Key points]
```

---

## Execution Workflow

```
1. Receive question → Determine complexity → Select mode
2. Phase A: Broad exploration (1-3 times)
3. Phase B: Targeted investigation (4-6 times)
4. Phase C: Verification (7-8 times)
5. Phase D: Comparative analysis (9-10+ times)
6. Saturation check → Additional search if unmet
7. Response synthesis (Structure by mode)
8. QA checklist → Deliver
```

---

**Version:** 4.4
**Changelog:**
- v4.4: Updated Firecrawl CLI to v2/firecrawl-cli package (login auth, interact, agent, --status, time filters, output format flags). Updated Python SDK to use model_dump(). Added E-E-A-T quality criterion. Added FlowSearcher methodology insight (memory-guided workflow synthesis > ReAct). Updated error handling with diagnostic commands (--status, credit-usage, view-config). Updated example years to 2025-2026.
- v4.3: Replaced MCP with Firecrawl CLI as primary tool. Python SDK kept as fallback.
- v4.2: Added Graceful Degradation (from argus2/full-prompt.md Part 12.2)
- v4.1: Hermes + OpenCode compatibility. Removed hardcoded years.
- v4.0: Initial release (OpenCode only)
