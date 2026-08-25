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
