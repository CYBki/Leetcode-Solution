# Longest Palindromic Substring - Detailed Solution

## Problem Statement

Given a string `s`, return the longest palindromic substring in `s`.

A palindrome is a string that reads the same forward and backward.

### Examples

```
Input: s = "babad"
Output: "bab"
Explanation: "aba" is also a valid answer.

Input: s = "cbbd"
Output: "bb"

Input: s = "racecar"
Output: "racecar"

Input: s = "a"
Output: "a"
```

## Solution

```python
class Solution:
    def longestPalindrome(self, s: str) -> str:
        # Initialize an empty string to store the result
        res = ""
        # Iterate through each character in the string
        for i in range(len(s)):
            # Check for both odd and even length palindromes centered at the current character
            for l, r in ((i,i), (i,i+1)):
                # Expand around the center to find the longest palindrome
                while l >= 0 and r < len(s) and s[l] == s[r]:
                    # Update the result if the current palindrome is longer
                    if (r - l + 1) > len(res):
                        res = s[l:r + 1]
                    # Expand the palindrome by moving the pointers
                    l -= 1
                    r += 1
        return res
```

## What is a Palindrome?

A palindrome reads the same forwards and backwards:
- "aba" → reversed "aba" ✓
- "racecar" → reversed "racecar" ✓
- "abc" → reversed "cba" ✗ (not a palindrome)

## Solution Approach - Expand Around Center

### Core Idea

For each possible center in the string:
1. Consider it as the middle of a potential palindrome
2. Expand outwards (left and right) while characters match
3. Keep track of the longest palindrome found

### Why Two Cases?

We need to handle both odd and even length palindromes:

**Odd Length Palindrome (single center)**
```
"a b a"
   ↑
 center
```

**Even Length Palindrome (center between two characters)**
```
"a b b a"
   ↑ ↑
 center
```

## Step-by-Step Walkthrough

Let's trace through `s = "babad"`:

### Initial State
```
res = ""
```

### Step 1: i=0, character='b'

**Case 1 - Odd length (l=0, r=0):**
```
"b a b a d"
 ↑
L,R
```
- s[0] == s[0] ✓ ('b' == 'b')
- Current palindrome: "b"
- res = "b" (length 1)
- Expand: l=-1, r=1
- l < 0, stop

**Case 2 - Even length (l=0, r=1):**
```
"b a b a d"
 ↑ ↑
 L R
```
- s[0] != s[1] ✗ ('b' != 'a')
- Stop immediately

### Step 2: i=1, character='a'

**Case 1 - Odd length (l=1, r=1):**
```
"b a b a d"
   ↑
  L,R
```
- s[1] == s[1] ✓ ('a' == 'a')
- Current palindrome: "a"
- Expand: l=0, r=2

```
"b a b a d"
 ↑   ↑
 L   R
```
- s[0] == s[2] ✓ ('b' == 'b')
- Current palindrome: "bab"
- res = "bab" (length 3 > 1)
- Expand: l=-1, r=3
- l < 0, stop

### Final Result: "bab"

## Code Explanation

```python
def longestPalindrome(self, s: str) -> str:
    res = ""  # Store the longest palindrome found
    
    for i in range(len(s)):  # Try each position as center
        
        for l, r in ((i,i), (i,i+1)):  # Two cases: odd and even
            
            # Expand while valid palindrome
            while l >= 0 and r < len(s) and s[l] == s[r]:
                # Check three conditions:
                # 1. l >= 0: left pointer within bounds
                # 2. r < len(s): right pointer within bounds
                # 3. s[l] == s[r]: characters match
                
                # Update result if current is longer
                if (r - l + 1) > len(res):
                    res = s[l:r + 1]
                
                # Expand outwards
                l -= 1
                r += 1
    
    return res
```

## Visual Example

```
s = "babad"

Position 0: "b"
Position 1: "a" → expand → "bab" ✓
Position 2: "b" → expand → "aba" (same length)
Position 3: "a"
Position 4: "d"

Longest found: "bab"
```

## Complexity Analysis

### Time Complexity: O(n²)

```python
for i in range(len(s)):           # O(n) - iterate through string
    for l, r in ((i,i), (i,i+1)): # O(2) = O(1) - always 2 cases
        while expanding:           # O(n) - worst case expands entire string
```

- Outer loop: O(n)
- Expansion for each center: O(n) worst case
- Total: O(n) × O(n) = O(n²)

### Space Complexity: O(1)

- Only using a few variables (i, l, r)
- `res` stores the output (not counted as extra space)
- No additional data structures

## Comparison with Other Approaches

| Approach | Time | Space | Pros | Cons |
|----------|------|-------|------|------|
| **Expand Around Center** | O(n²) | O(1) | Simple, space efficient | - |
| **Brute Force** | O(n³) | O(1) | Very simple | Too slow |
| **Dynamic Programming** | O(n²) | O(n²) | Systematic | High memory usage |
| **Manacher's Algorithm** | O(n) | O(n) | Fastest | Complex to implement |

## Alternative Solutions

### 1. Brute Force - Check All Substrings
```python
def longestPalindrome_brute(s):
    def isPalindrome(substr):
        return substr == substr[::-1]
    
    longest = ""
    for i in range(len(s)):
        for j in range(i, len(s)):
            if isPalindrome(s[i:j+1]):
                if len(s[i:j+1]) > len(longest):
                    longest = s[i:j+1]
    return longest
```

### 2. Dynamic Programming
```python
def longestPalindrome_dp(s):
    n = len(s)
    dp = [[False] * n for _ in range(n)]
    longest = ""
    
    # Every single character is palindrome
    for i in range(n):
        dp[i][i] = True
        longest = s[i]
    
    # Check for palindromes of length 2 and more
    for length in range(2, n + 1):
        for i in range(n - length + 1):
            j = i + length - 1
            
            if s[i] == s[j]:
                if length == 2 or dp[i+1][j-1]:
                    dp[i][j] = True
                    if length > len(longest):
                        longest = s[i:j+1]
    
    return longest
```

## Edge Cases

1. **Empty string**: Return ""
2. **Single character**: Return the character
3. **All same characters**: Return entire string
4. **No palindrome > 1**: Return any single character

## Key Insights

1. **Two pointers technique**: Efficiently expand from center
2. **Handle both odd and even**: Complete coverage
3. **Early termination**: Stop when boundaries reached
4. **Update only when longer**: Maintain the best result

## Practice Tips

1. Always consider both odd and even length palindromes
2. Be careful with string boundaries
3. Remember that single characters are palindromes
4. The expand around center approach is usually the best balance of simplicity and efficiency

## Related Problems

- Palindromic Substrings (count all palindromes)
- Longest Palindromic Subsequence
- Valid Palindrome
- Palindrome Partitioning

---

*The Expand Around Center approach provides an optimal O(n²) solution with minimal space usage, making it the preferred choice for interviews and practical applications.*