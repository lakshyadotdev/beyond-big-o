# 58. Length of Last Word

---

| | |
| --- | --- |
| **Difficulty** | Easy |
| **Topics** | _none_ |
| **Language** | C++ |
| **LeetCode** | [https://leetcode.com/problems/length-of-last-word/](https://leetcode.com/problems/length-of-last-word/) |
| **Status** | Accepted |
| **Runtime** | 0 ms (100%) |
| **Memory** | 8.9 MB (35.195299999999996%) |
| **Tests** | 60 / 60 |
| **Submitted** | 2026-07-29T11:21:04.000Z |

## Table of Contents

* [Problem Summary](#problem-summary)
* [Approach](#approach)
* [Step-by-step](#step-by-step)
* [Complexity](#complexity)
* [Solution](#solution)

---

## Problem Summary

Given a string `s` containing words separated by spaces, return the length of the last word. A word is a maximal substring of non-space characters. Trailing spaces may exist before the last word.
---

## Approach

Iterate the string from the end backwards. Skip trailing spaces until the first non-space character is found, then count consecutive non-space characters until a space is encountered or the beginning of the string is reached. This avoids splitting the string and runs in a single pass.
---

## Step-by-step

1. Declare `siz = s.size()`, `kount = 0`, and `flag = 0` to store the string length, the running character count, and a marker indicating whether a non-space character has been seen yet.
2. Loop `i` from `siz - 1` down to `0`, scanning the string backwards.
3. Check `if (s[i] == ' ' && flag)` — if the current character is a space and `flag` is already set (meaning we have started counting a word), `break` out of the loop since the word has ended.
4. Check `if (s[i] != ' ')` — if the current character is not a space, set `flag = 1` to mark that counting has begun and increment `kount` by 1.
5. After the loop finishes, `return kount`, which holds the length of the last word.
---

## Complexity

Time Complexity: O(n)
Space Complexity: O(1)
We traverse the string at most once from end to start, using only a few integer variables.

---

## Solution

### C++

```cpp
class Solution {
public:
    int lengthOfLastWord(string s) {
        int siz=s.size(),kount=0,flag=0;
        for(int i=siz-1;i>=0;i--){
            if(s[i]==' '&&flag)break;
            if(s[i]!=' '){
                flag=1;
                kount++;
            }
        }
        return kount;
    }
};

```
