---
name: dietly-food-search
description: Search DietlyAPI's 4.7M-food catalog and report confidence-ranked nutrition per 100g.
api: DietlyAPI
operations: [searchFoods]
generated: '2026-09-03'
method: generated
source: openapi/dietlyapi-openapi.json + https://www.getdietly.com/api-guide
---

# Search foods on DietlyAPI

1. Call `GET https://api.getdietly.com/search?q=<query>&limit=5` (operationId `searchFoods`). No key
   is required at 30 req/min per IP; send `Authorization: Bearer <key>` for per-account plan limits.
   Optional `source=off|usda|claude` filters by data source.
2. Validate locally first: `q` needs at least 2 characters and `limit` must be 1-50 — violations
   return a 422 whose `detail` is a LIST of validation objects, not a string.
3. The response is a BARE array (`[]` on no match), not `{"results": [...]}`. Results are ranked by
   relevance x confidence; the first element is the most trustworthy match.
4. Report values per 100g, name the product and brand, and pass through `source` and `confidence`.
   Missing nutrients are `null` — say "unavailable", never treat null as zero.
5. On 429, wait for the `Retry-After` header, then retry with capped exponential backoff.
6. Where the data is shown publicly, include the required one-line credit: "Food data from Open Food
   Facts (ODbL)".
