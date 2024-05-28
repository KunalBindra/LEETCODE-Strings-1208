# LEETCODE-Strings-1208
Let's walk through this implementation of the `equalSubstring` method step-by-step to understand how it works and why it achieves the desired result.

### Problem Statement
Given two strings `s` and `t` of the same length, and an integer `maxCost`, you need to find the maximum length of a substring that can be transformed from `s` to `t` such that the total cost of transformation does not exceed `maxCost`. The cost of transforming a character `s[i]` to `t[i]` is defined as `|s[i] - t[i]|`.

### Approach
The problem is tackled using a sliding window technique. Here's how it works in detail:

1. **Initialization**:
   - We start with a window defined by the pointers `i` (the end of the window) and `j` (the start of the window).
   - `maxCost` keeps track of the remaining allowable transformation cost.

2. **Expanding the Window**:
   - For each character at position `i` in the string, calculate the cost to transform `s[i]` to `t[i]` and subtract this cost from `maxCost`.

3. **Shrinking the Window**:
   - If `maxCost` becomes negative (i.e., the cost exceeds the allowable limit), increment `j` to shrink the window from the left. Add back the cost of the transformation for `s[j]` to `t[j]` to `maxCost`.

4. **Calculating the Result**:
   - The length of the current valid window is `i - j + 1`. However, we are interested in the length of the longest valid window by the end of the traversal. Since `i` runs from `0` to `s.length() - 1`, the length of the maximum valid window is `s.length() - j` when the loop ends.

### Dry Run
Let's go through an example to understand the execution flow.

- **Example Input**: `s = "abcd"`, `t = "bcdf"`, `maxCost = 3`
- **Cost Calculation**:
  - `|a - b| = 1`
  - `|b - c| = 1`
  - `|c - d| = 1`
  - `|d - f| = 2`
- **Steps**:
  1. Initialize: `j = 0`, `maxCost = 3`
  2. For `i = 0`:
     - `maxCost -= |a - b| = 3 - 1 = 2`
  3. For `i = 1`:
     - `maxCost -= |b - c| = 2 - 1 = 1`
  4. For `i = 2`:
     - `maxCost -= |c - d| = 1 - 1 = 0`
  5. For `i = 3`:
     - `maxCost -= |d - f| = 0 - 2 = -2`
     - Since `maxCost < 0`, increment `j` to 1 and adjust `maxCost`:
       - `maxCost += |a - b| = -2 + 1 = -1`
     - Since `maxCost < 0`, increment `j` to 2 and adjust `maxCost`:
       - `maxCost += |b - c| = -1 + 1 = 0`
  6. End of loop: `j = 2`, thus the longest valid substring length is `s.length() - j = 4 - 2 = 2`

### Final Code
Here is the code again with comments for clarity:

```java
class Solution {
  public int equalSubstring(String s, String t, int maxCost) {
    int j = 0;  // Start of the sliding window
    for (int i = 0; i < s.length(); ++i) {
      // Subtract the cost of transforming s[i] to t[i]
      maxCost -= Math.abs(s.charAt(i) - t.charAt(i));
      
      // If maxCost is negative, shrink the window from the left
      if (maxCost < 0) {
        // Add back the cost of transforming s[j] to t[j] and move j right
        maxCost += Math.abs(s.charAt(j) - t.charAt(j++));
      }
    }

    // The length of the longest valid substring
    return s.length() - j;
  }
}
```

This code effectively uses a sliding window to keep track of the longest substring that can be transformed within the given cost constraint, achieving the goal in linear time.
