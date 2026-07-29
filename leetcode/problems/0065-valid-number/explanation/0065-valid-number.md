# 65. Valid Number

---

| | |
| --- | --- |
| **Difficulty** | Hard |
| **Topics** | _none_ |
| **Language** | C++ |
| **LeetCode** | [https://leetcode.com/problems/valid-number/](https://leetcode.com/problems/valid-number/) |
| **Status** | Accepted |
| **Runtime** | 0 ms (100%) |
| **Memory** | 8.3 MB (25.73750000000001%) |
| **Tests** | 1499 / 1499 |
| **Submitted** | 2026-07-29T11:24:14.000Z |

## Table of Contents

* [Problem Summary](#problem-summary)
* [Approach](#approach)
* [Step-by-step](#step-by-step)
* [Complexity](#complexity)
* [Solution](#solution)

---

## Problem Summary

Determine whether a given string `s` represents a valid number, which may include an optional sign, digits, an optional decimal point, and an optional exponent part (`e`/`E` followed by an integer). Return `true` if valid, `false` otherwise.
---

## Approach

The solution uses a single-pass character scan with counters to track the occurrences of digits, decimal points, and exponent markers. It validates structural rules on the fly: signs must appear only at the start or right after `e`/`E`, there can be at most one `.` and one `e`/`E`, `.` cannot follow `e`, and digits must exist both before `e` and after `e` (if present).
---

## Step-by-step

1. Declare `int n = s.size()` and four counters: `countE`, `countD`, `countN`, `countAE` to track exponents, dots, digits before `e`, and digits after `e` respectively.
2. Loop `i` from `0` to `n - 1`, inspecting each character `s[i]`.
3. Increment `countD` if `s[i] == '.'`, increment `countE` if `s[i]` is `'e'` or `'E'`.
4. If `isdigit(s[i])`, increment `countAE` when `countE > 0` (after exponent), otherwise increment `countN` (before exponent).
5. If `s[i]` is `'+'` or `'-'` and it is not at index `0` and the previous character is not `'e'`/`'E'`, return `false` — signs are only valid at the start or right after an exponent marker.
6. If `isalpha(s[i])` and the character is not `'e'` or `'E'`, return `false` — no other letters are allowed.
7. If `countD > 1` or `countE > 1`, return `false` — at most one dot and one exponent marker are permitted.
8. If `s[i] == '.'` and `countE > 0`, return `false` — a dot cannot appear after the exponent.
9. If `i == 0` or `i == n - 1` and `s[i]` is `'e'`/`'E'`, return `false` — exponent marker cannot be at the very start or end.
10. If `i == 0 && i == n - 1 && s[i] == '.'`, return `false` — a single-character string `"."` is invalid.
11. If `s[i]` is `'e'`/`'E'` and `countN == 0`, return `false` — there must be at least one digit before the exponent.
12. After the loop, if `countE > 0 && countAE == 0` or `countN == 0`, return `false` — ensure digits exist after `e` (if present) and at least one digit exists overall.
13. Return `true` if all checks passed.
---

## Complexity

Time Complexity: O(n)
Space Complexity: O(1)
The string is traversed once with a fixed number of integer counters, so time is linear in the length of `s` and space is constant.

---

## Solution

### C++

```cpp
class Solution {
public:
    bool isNumber(string s) {
        int n = s.size();
        int countE = 0;   // count of 'e' or 'E'
        int countD = 0;   // count of '.'
        int countN = 0;   // count of digits before exponent
        int countAE = 0;  // count of digits after exponent

        for (int i = 0; i < n; i++) {
            if (s[i] == '.') countD++;
            if (s[i] == 'e' || s[i] == 'E') countE++;
            if (isdigit(s[i])) {
                if (countE) countAE++;
                else countN++;
            }
            // sign validation
            if ((s[i] == '+' || s[i] == '-') && i != 0 && (s[i - 1] != 'e' && s[i - 1] != 'E')) 
                return false;
            // invalid alphabet
            else if (isalpha(s[i]) && (s[i] != 'e' && s[i] != 'E')) 
                return false;
            // multiple '.' or 'e'
            else if (countD > 1 || countE > 1) 
                return false;
            // '.' cannot appear after 'e'
            else if (s[i] == '.' && countE) 
                return false;
            // 'e'/'E' cannot be at beginning or end
            else if ((i == 0 || i == n - 1) && (s[i] == 'e' || s[i] == 'E')) 
                return false;
            // single '.' cannot be both start and end
            else if (i == 0 && i == n - 1 && s[i] == '.') 
                return false;
            // 'e/E' must follow at least one digit
            else if ((s[i] == 'e' || s[i] == 'E') && !countN) 
                return false;
        }
        // validate digit presence
        if ((countE && !countAE) || !countN) return false;
        return true;
    }
};
```
