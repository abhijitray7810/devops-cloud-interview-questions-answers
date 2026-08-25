# Amazon Warehouse - Optimal Inventory (Question 1)

## Problem Statement
The manager of the Amazon warehouse has decided to make changes to the inventory. Currently, the inventory has `n` products, where the quality of the `i-th` product after quality checks is represented by `quality[i]`.

The manager wants to create an **optimal inventory**, where:
- **All occurrences of each quality value must be contiguous.**

### Operation
You can perform any number of times:
1. Choose two quality values `x` and `y`.
2. Replace every product with quality `x` to have quality `y` instead.
3. This operation costs `num_replacements` units, where `num_replacements` is the number of products whose quality was changed.

Find the minimum amount of money to convert the inventory into an optimal inventory.

> Note: Quality can be negative.

### Constraints
- `1 <= n <= 2 * 10^5`
- `-10^9 <= quality[i] <= 10^9`

### Examples
- `n=7, quality=[7,7,5,7,3,5,3]` -> `4`
- Sample 0: `[1,2,1,2,1]` -> `2` (replace all 2 -> 1)
- Sample 1: `[10,6,10,-3,1,1,4,-4,-1,1,-7]` -> `4`

### Function Signature
```python
def getMinAmount(quality: List[int]) -> int:
## Solution Approach - Weighted Interval Scheduling

1. *Frequency:* `freq[v]` = total count of value `v` in original array. This is the cost to delete `v`.
2. *Compression:* Remove consecutive duplicates: `[1][1][2][1]` -> `[1][2][1]`.
3. *Interval for each distinct value:* `first[v]` and `last[v]` in compressed array.
4. *Conflict Rule:* If we want to keep two values `a` and `b`, their intervals `[first][last]` must be *disjoint* (`last_a < first_b`). Otherwise one appears inside the other and contiguity breaks.
5. *Reduction:* We need to select a set of disjoint intervals with maximum total profit (`profit = freq[v]`). This is classic Weighted Interval Scheduling.
6. *DP:* Sort intervals by `last`.
    `dp[i] = max(dp[i-1], profit[i] + dp[prev])` where `prev` is last interval with `last[prev] < first[i]`.
7. *Answer:* `minCost = n - maxKeptProfit`

### Complexity
- Time: `O(n log n)`
- Space: `O(n)`

### Python 3 Implementation
import bisect
from collections import defaultdict
from typing import List

def getMinAmount(quality: List[int]) -> int:
    n = len(quality)
    if n == 0:
        return 0

    freq = defaultdict(int)
    for v in quality:
        freq[v] += 1

    comp = []
    for v in quality:
        if not comp or comp[-1]!= v:
            comp.append(v)

    first, last = {}, {}
    for i, v in enumerate(comp):
        if v not in first:
            first[v] = i
        last[v] = i

    intervals = [] # (last, first, profit)
    for v in first:
        intervals.append((last[v], first[v], freq[v]))

    intervals.sort()
    ends = [it[0] for it in intervals]

    dp = [0] * len(intervals)
    for i in range(len(intervals)):
        last_i, first_i, profit_i = intervals[i]
        j = bisect.bisect_left(ends, first_i) - 1
        take = profit_i + (dp[j] if j >= 0 else 0)
        not_take = dp[i-1] if i > 0 else 0
        dp[i] = take if take > not_take else not_take

    max_kept = dp[-1] if dp else 0
    return n - max_kept

The manager of the Amazon warehouse has decided to make changes to the inventory. Currently, the inventory has n products, where the quality of the i-th product after quality checks is represented by the array element quality[i].The manager wants to create an optimal inventory, where the array of products quality follows the following property:
_ All occurrences of each quality value must be contiguous.In order to convert the inventory into an optimal inventory, the manager can do the following operation any number of times:
Choose two quality values x and y.Replace every product with quality x to have quality y instead.This operation costs num_replacements units of money, where num_replacements is the number of products whose quality was changed.Given n products and an array quality, find the minimum amount of money the manager has to spend to convert the inventory into an optimal inventory.Note: The quality of a product can be negative.Example: n=7, quality=[7][7][5][7][3][5][3] -> answer 4
Sample 0: [1][2][1][2][1] -> 2
Sample 1: [10,6,10,-3,1,1,4,-4,-1,1,-7] -> 4Constraints: 1 <= n <= 2_10^5, -10^9 <= quality[i] <= 10^9



Tomar 2 number er question er full `README.md` ta niche dilam - tomar image theke full text niye banano:
# Question 2 - Fix The Browse Engine (S2)

## Context
When a customer opens a category page, like Electronics, in an E-commerce App, they expect to see everything under it: phones, cameras, smart home devices, etc.

Categories are organized in a parent-child hierarchy, so browsing a category must include products assigned to that category and every level beneath it, not just items tagged at the top level.

Results should also be region-specific and ranked by popularity. A customer in the US should only see US listings. Listings from other regions must never appear.

## Issue
The current browse engine is broken. It misses products from subcategories, surfaces products from unrelated categories, leaks listings from other regions, and returns results in the wrong order.

Your task is to fix the engine so it returns only the correct, in-region products for the requested category in the correct ranked order.

---

### How categories are organized
**Input file:** `data/categories.json`

This file contains a flat list of category records. Each record includes:
- `category_id` — unique identifier
- `name` — display name
- `parent_id` — parent category ID (null means it's a top-level category)

#### How to interpret it
You can think of it as chains like:
ELEC (Electronics)
  -> ELEC-CAM (Cameras & Photography)
  -> ELEC-SMART (Smart Home)
       -> ELEC-SMART-SEC (Security)

#### Example:
```json
{
  "categories": [
    {"category_id":"ELEC","name":"Electronics","parent_id": null},
    {"category_id":"ELEC-MOB","name":"Cell Phones","parent_id":"ELEC"},
    {"category_id":"ELEC-MOB-AND","name":"Android","parent_id":"ELEC-MOB"},
    {"category_id":"ELEC-CAM","name":"Cameras","parent_id":"ELEC"},
    {"category_id":"ELEC-SMART","name":"Smart Home","parent_id":"ELEC"},
    {"category_id":"ELEC-SMART-SEC","name":"Security","parent_id":"ELEC-SMART"},
    {"category_id":"HOME","name":"Home","parent_id": null},
    {"category_id":"HOME-APP","name":"Kitchen","parent_id":"HOME"}
  ]
}
Top-level categories (where `parent_id = null`) are separate. For example, ELEC and HOME are different top-level categories; browsing ELEC must never return anything from HOME or anything under HOME.

---

### Product listings
*Input file:* `data/products.jsonl`

One JSON object per line. Each line is one product listing.

Example line:
{"product_id":"P1001","name":"Galaxy Note","category_id":"ELEC-MOB-AND","region":"US","popularity_score": 95}
{"product_id":"P1003","name":"Sony ZV-E","category_id":"ELEC-CAM","region":"US","popularity_score": 98}
{"product_id":"P1004","name":"DoorGuard","category_id":"ELEC-SMART-SEC","region":"IN","popularity_score": 88}
### Browse requests
*Input file:* `data/requests.jsonl`

One JSON object per line. Each line is one browse request.

Each request includes:
- `query_id` — unique identifier
- `category_id` — the category being browsed
- `region` — region to filter by
- `max_results` — number of results to return; if missing, default to 5

Example:
{"query_id":"q1","category_id":"ELEC","region":"US","max_results": 2}
{"query_id":"q2","category_id":"ELEC-MOB","region":"US","max_results": 5}
### Output
*Output file:* `data/results.json`

A JSON array with one object per request, in the same order as the input.

Each output object must include:
- `query_id` — copied from input
- `matched_count` — number of eligible products after region filtering (before applying max_results)
- `products` — up to max_results products

Example:
{
  "query_id": "q1",
  "matched_count": 2,
  "products": [
    {"product_id":"P1001","name":"Galaxy Note", ...},
    {"product_id":"P1003","name":"Sony ZV-E", ...}
  ]
}
---

## Browse Engine Logic - Correct Implementation

Results are computed using the following steps, applied in order.

### Step 1: Collect included categories
Starting from the requested `category_id`, include:
- that category itself
- every subcategory under it (children, grandchildren, etc.)

If the `category_id` does not exist in the hierarchy, return `matched_count: 0` with an empty products list.

### Step 2: Filter eligible products
From products assigned to the included categories, keep only those where the region matches the request region exactly (case-sensitive string match).

After this step, the number of remaining products is `matched_count`.

### Step 3: Rank and return
Sort eligible products by:
- `popularity_score` (high -> low)
- `product_id` (low -> high) as tie-breaker

Return the first `max_results`.

---

## Fixed Solution (Python)
import json
from collections import defaultdict, deque

# 1. Load categories and build parent -> children map
with open('data/categories.json') as f:
    cat_data = json.load(f)['categories']

children = defaultdict(list)
all_ids = set()
for c in cat_data:
    all_ids.add(c['category_id'])
    if c['parent_id']:
        children[c['parent_id']].append(c['category_id'])

# 2. Function to get all descendants (including self)
def get_included(category_id):
    if category_id not in all_ids:
        return None
    included = set()
    q = deque([category_id])
    while q:
        cur = q.popleft()
        included.add(cur)
        for ch in children.get(cur, []):
            q.append(ch)
    return included

# 3. Load products
products = []
with open('data/products.jsonl') as f:
    for line in f:
        products.append(json.loads(line))

# Group products by category_id for speed
prod_by_cat = defaultdict(list)
for p in products:
    prod_by_cat[p['category_id']].append(p)

# 4. Process requests
results = []
with open('data/requests.jsonl') as f:
    for line in f:
        req = json.loads(line)
        qid = req['query_id']
        cat_id = req['category_id']
        region = req['region']
        max_res = req.get('max_results', 5)

        included = get_included(cat_id)
        if included is None:
            results.append({"query_id": qid, "matched_count": 0, "products": []})
            continue

        # Filter
        eligible = []
        for inc_cat in included:
            for p in prod_by_cat.get(inc_cat, []):
                if p['region'] == region: # case-sensitive exact match
                    eligible.append(p)
        
        matched_count = len(eligible)

        # Rank
        eligible.sort(key=lambda x: (-x['popularity_score'], x['product_id']))

        top = eligible[:max_res]

        results.append({
            "query_id": qid,
            "matched_count": matched_count,
            "products": top
        })

# 5. Write output
with open('data/results.json', 'w') as out:
    json.dump(results, out, indent=2)
