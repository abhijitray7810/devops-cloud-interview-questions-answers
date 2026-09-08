# GOCC139: Google's Online Challenge - IN 2027 Coding SAD Apprentice II

Test summary: 2 programming questions, both submitted successfully.

---

## Question 1: Lighthouse Flag Broadcast

### Problem Statement
Every evening, the keeper of the Saltmere lighthouse raises a sequence of signal flags along the harbour mast. Each flag contains one lowercase letter, and the flags remain in the order in which they were raised.

The glow strength of a flag is determined by its letter: `a = 1, b = 2, ..., z = 26`.

The harbour authority requires the keeper to select exactly `k` flags from the sequence without changing their original order. The total glow strength of the selected flags must be at least `f`.

Among all valid selections, determine the **lexicographically smallest string** formed by the selected flags.

### Function Signature
```
minFlagBroadcastCode(n, s, k, f)
```

### Input Format
1. Integer `n` — number of flags.
2. String `s` of `n` lowercase English letters.
3. Integer `k` — exact number of flags to select.
4. Integer `f` — minimum required total glow strength.

### Output Format
Print the lexicographically smallest valid broadcast string.

### Constraints
- `1 ≤ n ≤ 1,000,000`
- `1 ≤ k ≤ n`
- `s` contains only lowercase English letters, length exactly `n`.
- `0 ≤ f ≤` maximum total glow strength obtainable by selecting any `k` flags.
- At least one valid broadcast string is guaranteed to exist.

### Solution
```python
def minFlagBroadcastCode(n, s, k, f):
    count = [0] * 27
    total_sum = 0
    for ch in s:
        v = ord(ch) - 96
        count[v] += 1
        total_sum += v

    def max_sum_of_t(t):
        if t <= 0:
            return 0
        ans = 0
        for v in range(26, 0, -1):
            if count[v]:
                take = count[v]
                if take > t:
                    take = t
                ans += v * take
                t -= take
                if t == 0:
                    break
        return ans

    ans = []
    start = 0
    remaining = k
    required = f

    while remaining > 0:
        last = n - remaining
        best_char = None
        best_pos = -1
        scanned = []  # (position, value) pairs tentatively removed

        for i in range(start, last + 1):
            v = ord(s[i]) - 96
            count[v] -= 1
            total_sum -= v
            scanned.append((i, v))

            need = required - v
            if need <= 0:
                feasible = True
            else:
                max_possible = max_sum_of_t(remaining - 1)
                feasible = max_possible >= need

            if feasible:
                if best_char is None or v < best_char:
                    best_char = v
                    best_pos = i
                    if v == 1:
                        break

        # restore counts for scanned positions AFTER the chosen one —
        # they're still available for future rounds
        for pos, v in scanned:
            if pos > best_pos:
                count[v] += 1
                total_sum += v

        ans.append(s[best_pos])
        start = best_pos + 1
        remaining -= 1
        required -= best_char

    return ''.join(ans)


n = int(input())
s = input()
k = int(input())
f = int(input())
out_ = minFlagBroadcastCode(n, s, k, f)
print(out_)
```

### Approach
Greedy + feasibility check. At each step we try to pick the smallest possible letter at the earliest feasible position, as long as it's still possible to reach the required sum `f` with the remaining picks (checked via `max_sum_of_t`, the max achievable sum using `t` more picks from the remaining letter pool). Counting array + sum tracking keeps updates O(26) per step.

---

## Question 2: Concord Interval Census

### Problem Statement
A geophysics team records `n` consecutive pressure readings from a borehole into an array `a`, where `a[i]` is the reading at depth index `i`. Analysts study contiguous stretches of the log.

For every stretch, its **spread** is the difference between the largest and smallest reading inside it. A single reading forms a stretch of spread 0.

A stretch is **concordant** when its spread is at least `lo` and at most `hi`.

Given the readings and the two bounds `lo` and `hi`, count how many contiguous stretches are concordant. Two stretches covering different index ranges are counted separately even if their contents are identical.

### Function Signature
```
countSpreadWindows(n, lo, hi, a)
```

### Parameters
- `n`: number of readings.
- `lo`: minimum allowed spread (inclusive).
- `hi`: maximum allowed spread (inclusive).
- `a`: array of `n` integers, the pressure readings.

### Returns
A single integer: the number of concordant stretches.

### Input Format
1. Integer `n`.
2. Integer `lo`.
3. Integer `hi`.
4. `n` space-separated integers `a[0], a[1], ..., a[n-1]`.

### Output Format
A single integer: the number of contiguous stretches whose spread lies in `[lo, hi]`.

### Constraints
- `1 ≤ n ≤ 1,000,000`
- `0 ≤ lo ≤ hi ≤ 1,000,000,000`
- `1 ≤ a[i] ≤ 1,000,000,000`

### Sample
**Input**
```
5
0
2
1 3 2 4 3
```
**Output**
```
13
```

### Solution
```python
from collections import deque

def countSpreadWindows(n, lo, hi, a):

    def at_most(x):
        # count subarrays with (max - min) <= x
        if x < 0:
            return 0
        count = 0
        left = 0
        max_deque = deque()  # indices, decreasing values
        min_deque = deque()  # indices, increasing values

        for right in range(n):
            while max_deque and a[max_deque[-1]] <= a[right]:
                max_deque.pop()
            max_deque.append(right)

            while min_deque and a[min_deque[-1]] >= a[right]:
                min_deque.pop()
            min_deque.append(right)

            while a[max_deque[0]] - a[min_deque[0]] > x:
                if max_deque[0] == left:
                    max_deque.popleft()
                if min_deque[0] == left:
                    min_deque.popleft()
                left += 1

            count += right - left + 1

        return count

    return at_most(hi) - at_most(lo - 1)


n = int(input())
lo = int(input())
hi = int(input())
a = list(map(int, input().split()))
print(countSpreadWindows(n, lo, hi, a))
```

### Approach
Direct counting of subarrays with a spread condition is hard, so we use the standard trick:

```
count(spread in [lo, hi]) = count(spread <= hi) - count(spread <= lo - 1)
```

`at_most(x)` counts subarrays with `max - min <= x` using a **two-pointer sliding window** with two monotonic deques (one tracking the max, one tracking the min) — this runs in O(n) since each index enters/exits each deque once. For a fixed `right`, as `left` increases the window only shrinks, so the whole scan is linear. Total complexity: O(n) for each of the two calls, so O(n) overall — well within limits for `n ≤ 1,000,000`.

**Verification with sample:** `n=5, lo=0, hi=2, a=[1,3,2,4,3]` → `at_most(2) - at_most(-1) = 15 - 2 = 13` ✅ matches expected output.

---

## Suggested Next Steps (per Google's email)
- No online posting of results — Google's team will reach out only if selected to proceed.
- Test results/questions are Google's confidential IP — keep this README private, don't share externally.
- Wait for further communication from `googleonlinechallenge-ticket@google.com`.
