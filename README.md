# Argus — Deep Web Research Skill 🔍

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![Compatibility: Hermes Agent + OpenCode](https://img.shields.io/badge/Compatibility-Hermes%20Agent%20|%20OpenCode-blue)]()
[![Version](https://img.shields.io/badge/version-4.4--r1-brightgreen)]()

**Argus** is a systematic deep web research skill for AI agents. It goes beyond single-shot search — using a **4-Phase Framework** to explore, investigate, cross-verify, and synthesize information from multiple sources.

Built for [Hermes Agent](https://hermes-agent.nousresearch.com) and [OpenCode](https://github.com/opencode-ai/opencode).

---

## 🔑 Key Features

- **4-Phase Search Framework:** Broad exploration → Targeted investigation → Cross-source verification → Comparative synthesis
- **Source Transparency:** Every claim is cited as `[web:N]` with a full Source Transparency Report
- **Quantified Evidence:** No vague claims — only specific metrics with citations
- **3-way Firecrawl Access:** CLI (recommended) / Python SDK / Direct HTTP — works in any environment
- **Graceful Degradation:** 4 fallback levels when search tools fail
- **Quality Scoring:** Point-based source evaluation with E-E-A-T criteria

---

## 🚀 Quick Start

```bash
# For Hermes Agent
hermes skills install research:argus

# For OpenCode
# Copy SKILL.md and references/ to your skills directory
```

### Prerequisites

Argus uses [Firecrawl](https://firecrawl.dev) for web search and content extraction:

```bash
# Option 1: CLI (recommended)
npm install -g firecrawl-cli
firecrawl login --api-key fc-your-api-key

# Option 2: Python SDK
pip install firecrawl-py
export FIRECRAWL_API_KEY="your-api-key"
```

---

## 📖 How It Works

### 4-Phase Framework

```
Phase A: Broad Exploration  ──→  Phase B: Targeted Investigation  ──→  Phase C: Cross-Source Verification  ──→  Phase D: Comparative Synthesis
  (1-3 searches)                 (4-6 searches)                       (7-8 searches)                           (9-10+ searches)
```

| Phase | Purpose | Key Activities |
|-------|---------|----------------|
| **A** | Understand landscape | Identify core concepts, current state, key players |
| **B** | Gather specifics | Collect metrics, implementation details, common problems |
| **C** | Verify everything | Cross-check 2+ independent sources, flag conflicts |
| **D** | Compare & synthesize | Compare alternatives, identify gaps, outlook |

### Response Modes

| Mode | Trigger | Structure |
|------|---------|-----------|
| **A: Implementation** | "how to", "setup" | Quick Start → Steps → Config → Troubleshooting |
| **B: Architecture** | "how it works", "compare" | Direct Answer → Components → Comparison → Deep Dive |
| **C: Research** | "analyze", "recommend" | Executive Summary → Analysis → Alternatives → Roadmap |

---

## ⚙️ Error Handling

When search tools fail, Argus degrades gracefully:

| Level | Condition | Min Searches |
|-------|-----------|-------------|
| 1: Full | All tools working | Standard |
| 2: Partial | Some tools failing | 5 |
| 3: Minimal | Most tools failing | 0 (built-in knowledge) |
| 4: None | No external access | 0 (inform user) |

---

## 📂 Repository Structure

```
argus/
├── SKILL.md                  ← Main agent skill file (router)
├── LICENSE                   ← MIT License
├── README.md                 ← This file
└── references/
    ├── tool-setup.md         ← Firecrawl setup & usage
    ├── search-framework.md   ← Full 4-Phase Framework
    ├── quality-standards.md  ← Quality scoring & evaluation
    └── firecrawl-python-sdk-quirks.md  ← SDK pitfalls reference
```

---

## 📄 License

This project is licensed under the MIT License — see the [LICENSE](LICENSE) file for details.

---

**Version:** 4.4-r1
