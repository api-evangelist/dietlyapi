---
name: dietly-barcode-lookup
description: Resolve an EAN-13/UPC-A barcode to one nutrition record, treating misses as empty state.
api: DietlyAPI
operations: [barcodeLookup, getFood]
generated: '2026-09-03'
method: generated
source: openapi/dietlyapi-openapi.json + https://www.getdietly.com/developers/error-handling/
---

# Look up a barcode on DietlyAPI

1. Call `GET https://api.getdietly.com/barcode/{code}` (operationId `barcodeLookup`) with an EAN-13
   or UPC-A code. A key is optional for reads.
2. A hit returns ONE food object with per-100g macros, micronutrients, `source` and `confidence`.
   Store the record's `id` and use `GET /food/{food_id}` (operationId `getFood`) for later re-reads
   — food IDs are stable.
3. A miss returns `404 {"detail": "Barcode not found"}` — an ordinary outcome for a scanner, not a
   failure. Show an empty state, never an error dialog. (Contrast: `/search` with no match returns
   `200 []`.)
4. A 401 on this endpoint is nearly always a path typo, because reads need no key and auth runs
   before routing (`/v1/barcode/...` returns 401, not 404).
5. On 429, honour `Retry-After`. Cache hits by barcode — negative-cache 404s briefly to save quota.
