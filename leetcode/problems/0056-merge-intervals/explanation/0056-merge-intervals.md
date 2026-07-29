# 56. Merge Intervals

---

| | |
| --- | --- |
| **Difficulty** | Medium |
| **Topics** | _none_ |
| **Language** | C++ |
| **LeetCode** | [https://leetcode.com/problems/merge-intervals/](https://leetcode.com/problems/merge-intervals/) |
| **Status** | Accepted |
| **Runtime** | 8 ms (23.669999999999995%) |
| **Memory** | 23.9 MB (30.953799999999998%) |
| **Tests** | 172 / 172 |
| **Submitted** | 2026-07-29T11:17:01.000Z |

## Table of Contents

* [Problem Summary](#problem-summary)
* [Approach](#approach)
* [Step-by-step](#step-by-step)
* [Complexity](#complexity)
* [Solution](#solution)

---

## Problem Summary

Given an array of intervals `[start_i, end_i]`, merge all overlapping intervals and return the resulting non-overlapping intervals. Two intervals overlap if one's start is less than or equal to the other's end.
---

## Approach

Sort the intervals by their start time, then iterate through them while maintaining a `prev` interval. If the current interval overlaps with `prev`, extend `prev`'s end to the maximum of both ends. Otherwise, push `prev` into the result and start a new `prev`. This is a greedy, single-pass approach after sorting.
---

## Step-by-step

1. `sort(intervals.begin(), intervals.end())` sorts all intervals in ascending order by start time (and by end time as a tiebreaker).
2. `vector> merged` declares the result container for non-overlapping intervals.
3. `vector prev = intervals[0]` initializes the running interval to the first sorted interval.
4. `for (int i = 1; i < intervals.size(); i++)` iterates over every remaining interval starting from index 1.
5. `if (intervals[i][0] <= prev[1])` checks whether the current interval's start overlaps with or touches `prev`'s end.
6. `prev[1] = max(prev[1], intervals[i][1])` extends `prev`'s end to cover the current interval when they overlap.
7. `merged.push_back(prev)` appends the completed `prev` interval to the result when no overlap is found.
8. `prev = intervals[i]` resets the running interval to the current one for the next comparison.
9. `merged.push_back(prev)` after the loop, pushes the final accumulated interval into the result.
10. `return merged` returns the fully merged list of non-overlapping intervals.
---

## Complexity

Time Complexity: O(n log n)
Sorting dominates the linear scan. Space Complexity: O(n)
Output vector stores up to n intervals in the worst case (no overlaps).

---

## Solution

### C++

```cpp
class Solution {
public:
    vector<vector<int>> merge(vector<vector<int>>& intervals) {
        sort(intervals.begin(), intervals.end());
        vector<vector<int>> merged;
        vector<int> prev = intervals[0];

        for (int i = 1; i < intervals.size(); i++) {
            if (intervals[i][0] <= prev[1]) {
                // Merge overlapping intervals
                prev[1] = max(prev[1], intervals[i][1]);
            } else {
                merged.push_back(prev);
                prev = intervals[i];
            }
        }

        merged.push_back(prev);
        return merged;
    }
};
```
