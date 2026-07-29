# 23. Merge k Sorted Lists

---

| | |
| --- | --- |
| **Difficulty** | Hard |
| **Topics** | _none_ |
| **Language** | C++ |
| **LeetCode** | [https://leetcode.com/problems/merge-k-sorted-lists/](https://leetcode.com/problems/merge-k-sorted-lists/) |
| **Status** | Accepted |
| **Runtime** | 4 ms (42.285799999999995%) |
| **Memory** | 24 MB (5.011899999999976%) |
| **Tests** | 134 / 134 |
| **Submitted** | 2026-07-29T11:07:08.000Z |

## Table of Contents

* [Problem Summary](#problem-summary)
* [Approach](#approach)
* [Step-by-step](#step-by-step)
* [Complexity](#complexity)
* [Solution](#solution)

---

## Problem Summary

Given an array of `k` sorted linked-lists, merge them into a single sorted linked-list and return its head. The input is a `vector` and the output is a `ListNode*` representing the merged list.
---

## Approach

The solution uses a **divide-and-conquer** strategy similar to merge sort. It recursively splits the array of lists into two halves, merges each half, and then merges the two resulting sorted lists using a standard two-pointer merge. This avoids the O(N·k) cost of merging lists one-by-one and achieves O(N log k) time.
---

## Step-by-step

1. `mergeKLists` checks `if (lists.empty())` and returns `nullptr` to handle the edge case of zero input lists.
2. Otherwise it calls `mergeKListsHelper(lists, 0, lists.size() - 1)` to begin the recursive divide-and-conquer on the full range.
3. In `mergeKListsHelper`, if `start == end`, the single list `lists[start]` is returned as the base case.
4. If `start + 1 == end`, the two adjacent lists are merged directly via `merge(lists[start], lists[end])`.
5. Otherwise, `mid = start + (end - start) / 2` splits the range into two halves.
6. `left = mergeKListsHelper(lists, start, mid)` recursively merges the left half.
7. `right = mergeKListsHelper(lists, mid + 1, end)` recursively merges the right half.
8. `merge(left, right)` combines the two sorted halves and returns the result.
9. In `merge`, `dummy = new ListNode(0)` creates a sentinel node and `curr = dummy` tracks the tail of the merged list.
10. The `while (l1 && l2)` loop compares `l1->val` and `l2->val`, appending the smaller node to `curr->next` and advancing that pointer.
11. After the loop, `curr->next = l1 ? l1 : l2` attaches whichever list still has remaining nodes.
12. `return dummy->next` returns the head of the merged list, skipping the dummy sentinel.
---

## Complexity

Time Complexity: O(N log k) where N is the total number of nodes and k is the number of lists, because each level of recursion processes all N nodes and there are log k levels.
Space Complexity: O(log k) for the recursion stack depth, as the divide-and-conquer tree has height log k.

---

## Solution

### C++

```cpp
class Solution {
public:
    ListNode* mergeKLists(vector<ListNode*>& lists) {
        if (lists.empty()) {
            return nullptr;
        }
        return mergeKListsHelper(lists, 0, lists.size() - 1);
    }
    
    ListNode* mergeKListsHelper(vector<ListNode*>& lists, int start, int end) {
        if (start == end) {
            return lists[start];
        }
        if (start + 1 == end) {
            return merge(lists[start], lists[end]);
        }
        int mid = start + (end - start) / 2;
        ListNode* left = mergeKListsHelper(lists, start, mid);
        ListNode* right = mergeKListsHelper(lists, mid + 1, end);
        return merge(left, right);
    }
    
    ListNode* merge(ListNode* l1, ListNode* l2) {
        ListNode* dummy = new ListNode(0);
        ListNode* curr = dummy;
        
        while (l1 && l2) {
            if (l1->val < l2->val) {
                curr->next = l1;
                l1 = l1->next;
            } else {
                curr->next = l2;
                l2 = l2->next;
            }
            curr = curr->next;
        }
        
        curr->next = l1 ? l1 : l2;
        
        return dummy->next;
    }
};

```

---

*Captured by Leetcode-Auto at 2026-07-29T11:07:12.943Z*
