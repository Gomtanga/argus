# 🔧 Firecrawl Tool Setup

Argus uses **Firecrawl** for web search and content extraction. Three access methods, in order of preference.

## Method 1: CLI (Recommended)

```bash
# Install
npm install -g firecrawl-cli

# Authenticate
export FIRECRAWL_API_KEY="fc-your-api-key"
export FIRECRAWL_BASE_URL="https://api.firecrawl.dev"
firecrawl login --api-key "$FIRECRAWL_API_KEY"

# Verify
firecrawl --status
```

### Essential Commands

| Command | Purpose |
|---------|---------|
| `firecrawl search <query>` | Web search (returns markdown) |
| `firecrawl search <query> --scrape` | Search + auto-scrape results |
| `firecrawl search <query> --tbs qdr:y` | Search with recency filter (y/month/w) |
| `firecrawl search <query> --limit 10` | Control result count |
| `firecrawl scrape <url>` | Extract page content as markdown |
| `firecrawl credit-usage` | Check remaining credits |
| `firecrawl --status` | Auth/concurrency/credits status |

> 💡 **Search with time filter:** `--tbs qdr:y` = past year, `qdr:m` = month, `qdr:w` = week
> 💡 **Switch languages:** Search in English and Korean for broader coverage
> 💡 **Query-specific:** `"exact phrase"`, `site:domain.com`, `-exclude_term`

## Method 2: Python SDK (Fallback)

```python
from firecrawl import FirecrawlApp
from dotenv import load_dotenv
import os

load_dotenv(".env")  # ⚠️ Must load .env explicitly!

app = FirecrawlApp(
    api_key=os.getenv("FIRECRAWL_API_KEY"),
    api_url=os.getenv("FIRECRAWL_BASE_URL"),
)

# Search — ⚠️ Always call .model_dump()!
result = app.search(query="search term", limit=5)
data = result.model_dump()
items = data.get("web") or []     # Can be None!
news = data.get("news") or []     # Can be None!

# Scrape — use v1 for dict-like responses
page = app.v1.scrape_url("https://example.com")
content = page.model_dump()
markdown = content.get("markdown", "")
```

> ⚠️ **Pitfall:** `data["news"]` can be `None`, NOT an empty list. Always use `.get("news") or []`.
> ⚠️ **Pitfall:** `app.search()` and `app.scrape()` return Pydantic models, not plain dicts. Always call `.model_dump()` to access fields.

## Method 3: Direct HTTP (Zero Dependencies)

When neither CLI nor SDK is available:

```python
import json, urllib.request

API_KEY = "fc-your-api-key"
BASE_URL = "https://api.firecrawl.dev"

def _req(endpoint, payload):
    req = urllib.request.Request(
        f"{BASE_URL}/{endpoint}",
        data=json.dumps(payload).encode(),
        headers={
            "Content-Type": "application/json",
            "Authorization": f"Bearer {API_KEY}",
        },
        method="POST",
    )
    with urllib.request.urlopen(req, timeout=45) as resp:
        return json.loads(resp.read().decode())

def search(query, limit=5):
    result = _req("v1/search", {"query": query, "limit": limit})
    return (result.get("data") or result.get("web")) or []

def scrape(url):
    result = _req("v1/scrape", {"url": url, "formats": ["markdown"]})
    if "data" in result and isinstance(result["data"], dict):
        return result["data"].get("markdown", "")
    return ""
```

## Troubleshooting

| Symptom | Check | Fix |
|---------|-------|-----|
| Auth failures | `firecrawl view-config` | Re-login: `firecrawl login --api-key <key>` |
| Rate limited | `firecrawl credit-usage` | Reduce query count, wait |
| No results | Query too narrow/specific | Simplify → try EN+KR |
| SDK import fails | pip not installed | Use Method 3 (HTTP) instead |
| `.env` not loaded | SDK doesn't auto-load .env | Call `load_dotenv()` explicitly |
