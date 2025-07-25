# Valid Palindrome Solution

## Problem Description

Given a string `s`, return `true` if it is a palindrome, or `false` otherwise.

A phrase is a palindrome if, after converting all uppercase letters into lowercase letters and removing all non-alphanumeric characters, it reads the same forward and backward. Alphanumeric characters include letters and numbers.

## Examples

```
Input: s = "A man, a plan, a canal: Panama"
Output: true
Explanation: "amanaplanacanalpanama" is a palindrome.

Input: s = "race a car"
Output: false
Explanation: "raceacar" is not a palindrome.

Input: s = " "
Output: true
Explanation: s is an empty string "" after removing non-alphanumeric characters.
Since an empty string reads the same forward and backward, it is a palindrome.
```

## Solution 1: Clean and Compare

### Approach
1. Clean the string by keeping only alphanumeric characters and converting to lowercase
2. Compare the cleaned string with its reverse

### Implementation

```python
class Solution:
    def isPalindrome(self, s: str) -> bool:
        # Clean the string: keep only alphanumeric chars and convert to lowercase
        cleaned = ''.join(char.lower() for char in s if char.isalnum())
        
        # Compare with its reverse
        return cleaned == cleaned[::-1]
```

### Complexity Analysis
- **Time Complexity:** O(n), where n is the length of the input string
  - Cleaning the string: O(n)
  - Creating reverse: O(n)
  - Comparison: O(n)
- **Space Complexity:** O(n) for storing the cleaned string and its reverse

## Solution 2: Two Pointers (Space Optimized)

### Approach
Use two pointers from both ends, skip non-alphanumeric characters, and compare characters directly without creating a new string.

### Implementation

```python
class Solution:
    def isPalindrome(self, s: str) -> bool:
        left, right = 0, len(s) - 1
        
        while left < right:
            # Find next alphanumeric character from left
            while left < right and not s[left].isalnum():
                left += 1
            
            # Find next alphanumeric character from right
            while left < right and not s[right].isalnum():
                right -= 1
            
            # Compare characters (case insensitive)
            if s[left].lower() != s[right].lower():
                return False
            
            left += 1
            right -= 1
        
        return True
```

### Complexity Analysis
- **Time Complexity:** O(n), where n is the length of the input string
- **Space Complexity:** O(1), only using a constant amount of extra space

## Key Concepts

1. **String Preprocessing:** Removing unwanted characters and normalizing case
2. **Palindrome Check:** Comparing string with its reverse or using two pointers
3. **Space-Time Tradeoff:** Solution 1 is more intuitive but uses more space, Solution 2 is more space-efficient

## Related Problems
- Palindrome Number
- Valid Palindrome II
- Longest Palindromic Substring

## Tags
`String` `Two Pointers` `Palindrome` `Easy`