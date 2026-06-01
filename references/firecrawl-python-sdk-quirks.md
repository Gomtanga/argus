# Firecrawl Python SDK (4.x) — Quirks & Pitfalls

Discovered while testing against Firecrawl 4.22.2. Covers SDK class naming, response shapes, and common gotchas.

## Critical: API v2 vs v1 Response Shapes

### `scrape()` → `scrape_url()` — different methods

| Version | Method | Returns | Access pattern |
|---------|--------|---------|----------------|
| v2 (default `firecrawl.scrape()`) | `firecrawl.scrape(url)` | Pydantic `Document` model | `doc.model_dump()["markdown"]` |
| v1 (compatibility) | `firecrawl.v1.scrape_url(url)` | Pydantic `V1ScrapeResponse` model | `resp.model_dump()["markdown"]` |

**Pitfall:** Both return Pydantic model objects, NOT plain dicts. 속성 접근을 우선 사용하고 dict 변환이 필요하면 `.model_dump()` 사용.

```python
# Preferred: attribute access
markdown = resp.markdown

# When dict conversion is needed
data = resp.model_dump()
markdown = data.get("markdown", "")
```

### `search()` — same in v1/v2

```python
result = firecrawl.search(query, limit=2)
# Preferred: attribute access
web_results = result.web or []       # attribute access
# When dict conversion is needed:
data = result.model_dump()
web_results = data.get("web", [])     # list of dicts
news_results = data.get("news") or [] # CAN BE None — handle this!
```

**Pitfall:** `data["news"]` can be `None`, not an empty list. Always use `data.get("news") or []`.

## `.env` Loading

Firecrawl SDK reads `FIRECRAWL_API_KEY` and `FIRECRAWL_API_URL` from the environment. However:

```python
# THIS FAILS — .env is NOT auto-loaded
firecrawl = Firecrawl(api_key=os.getenv("FIRECRAWL_API_KEY"))

# YOU MUST explicitly load .env first
from dotenv import load_dotenv
load_dotenv("/path/to/.env")

firecrawl = Firecrawl(
    api_key=os.getenv("FIRECRAWL_API_KEY"),
    api_url=os.getenv("FIRECRAWL_API_URL")
)
```

**Pitfall:** The SDK does NOT call `load_dotenv()` internally. If running inside Hermes where the venv is activated but `.env` isn't sourced, you must load it yourself.

## Field: `.title` in scrape results

Despite having a `title` field in the response model, `scrape_url("https://example.com/")` returns `title: None` because that page has no explicit `<title>` tag. This is NOT a bug — example.com legitimately has no title.

## Memory cleanup

The `.env` file must exist in the working directory OR be explicitly loaded. The `.env` file should be in your project root. A common failure mode is the file being deleted or not persisted across sessions.

## Reference: Quick-Start Template

```python
from firecrawl import Firecrawl
from dotenv import load_dotenv
import os

load_dotenv(".env")

firecrawl = Firecrawl(
    api_key=os.getenv("FIRECRAWL_API_KEY"),
    api_url=os.getenv("FIRECRAWL_API_URL"),
)

# Scrape — preferred: attribute access; .model_dump() when dict needed
result = firecrawl.v1.scrape_url("https://example.com")
markdown = result.markdown           # preferred attribute access
# data = result.model_dump()        # use when dict conversion is needed
# markdown = data.get("markdown", "")

# Search — preferred: attribute access; .model_dump() when dict needed
search = firecrawl.search("query", limit=5)
for item in (search.web or []):      # preferred attribute access
    print(item.title, item.url)
# sd = search.model_dump()           # use when dict conversion is needed
# for item in (sd.get("web") or []):
#     print(item["title"], item["url"])
```
