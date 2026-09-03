---
name: dietly-food-discovery
description: Populate discovery and empty-state UIs from DietlyAPI popular foods and categories.
api: DietlyAPI
operations: [popularFoods, listCategories, health]
generated: '2026-09-03'
method: generated
source: openapi/dietlyapi-openapi.json + https://www.getdietly.com/api-guide
---

# Discover foods and categories on DietlyAPI

1. `GET https://api.getdietly.com/foods/popular` (operationId `popularFoods`) returns a bare array
   of curated common foods — designed for empty-state UIs before the user types a query.
2. `GET /foods/categories` (operationId `listCategories`) returns every food category with counts;
   a `FoodResult.category` is the category name string.
3. `GET /health` (operationId `health`) is the unauthenticated liveness probe (also reports
   `foods_in_db`); use it for availability checks, never for data.
4. Use `image_thumb_url` in lists and `image_url` for larger views.
5. These endpoints share the same limits as search: 30/min per IP anonymous, per-account with a key;
   429 carries `Retry-After`.
