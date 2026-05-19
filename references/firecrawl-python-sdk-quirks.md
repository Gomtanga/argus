# Firecrawl Python SDK (4.x) — Quirks & Pitfalls

Discovered while testing against Firecrawl 4.22.2 with Jiminbox endpoint at `https://firecrawl.jiminbox.com`.

## Critical: API v2 vs v1 Response Shapes

### `scrape()` → `scrape_url()` — different methods

| Version | Method | Returns | Access pattern |
|---------|--------|---------|----------------|
| v2 (default `app.scrape()`) | `app.scrape(url)` | Pydantic `Document` model | `doc.model_dump()["markdown"]` |
| v1 (compatibility) | `app.v1.scrape_url(url)` | Pydantic `V1ScrapeResponse` model | `resp.model_dump()["markdown"]` |

**Pitfall:** Both return Pydantic model objects, NOT plain dicts. Always call `.model_dump()` to access fields as dict.

### `search()` — same in v1/v2

```python
result = app.search(query, limit=2)
data = result.model_dump()
web_results = data.get("web", [])     # list of dicts
news_results = data.get("news") or [] # CAN BE None — handle this!
```

**Pitfall:** `data["news"]` can be `None`, not an empty list. Always use `data.get("news") or []`.

## `.env` Loading

Firecrawl SDK reads `FIRECRAWL_API_KEY` and `FIRECRAWL_BASE_URL` from the environment. However:

```python
# THIS FAILS — .env is NOT auto-loaded
app = FirecrawlApp(api_key=os.getenv("FIRECRAWL_API_KEY"))

# YOU MUST explicitly load .env first
from dotenv import load_dotenv
load_dotenv("/path/to/.env")

app = FirecrawlApp(
    api_key=os.getenv("FIRECRAWL_API_KEY"),
    api_url=os.getenv("FIRECRAWL_BASE_URL")
)
```

**Pitfall:** The SDK does NOT call `load_dotenv()` internally. If running inside Hermes where the venv is activated but `.env` isn't sourced, you must load it yourself.

## Field: `.title` in scrape results

Despite having a `title` field in the response model, `scrape_url("https://example.com/")` returns `title: None` because that page has no explicit `<title>` tag. This is NOT a bug — example.com legitimately has no title.

## Memory cleanup

The `.env` file must exist in the working directory OR be explicitly loaded. The Hermes `.env` location is `/opt/hermes/.env` (project root). A common failure mode is the file being deleted or not persisted across sessions.

## Reference: Quick-Start Template

```python
from firecrawl import FirecrawlApp
from dotenv import load_dotenv
import os

load_dotenv("/opt/hermes/.env")

app = FirecrawlApp(
    api_key=os.getenv("FIRECRAWL_API_KEY"),
    api_url=os.getenv("FIRECRAWL_BASE_URL"),
)

# Scrape
result = app.v1.scrape_url("https://example.com")
data = result.model_dump()
markdown = data.get("markdown", "")

# Search
search = app.search("query", limit=5)
sd = search.model_dump()
for item in (sd.get("web") or []):
    print(item["title"], item["url"])
```
