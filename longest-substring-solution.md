# Longest Substring Without Repeating Characters - Detailed Solution

## Problem Statement

Given a string `s`, find the length of the longest substring without repeating characters.

### Examples

```
Input: s = "abcabcbb"
Output: 3
Explanation: The answer is "abc", with the length of 3.

Input: s = "bbbbb"
Output: 1
Explanation: The answer is "b", with the length of 1.

Input: s = "pwwkew"
Output: 3
Explanation: The answer is "wke", with the length of 3.
```

## Solution

```python
class Solution:
    def lengthOfLongestSubstring(self, s: str) -> int:
        window = set()  # Set to store characters in the current window
        max_len = 0      # Initialize the maximum length of the substring
        left = 0         # Left pointer of the sliding window
        for right, char in enumerate(s):
            # Slide the window to the right until there are no repeating characters
            while char in window:
                window.remove(s[left])
                left += 1
            # Add the current character to the window
            window.add(char)
            # Update the maximum length of the substring
            max_len = max(max_len, right - left + 1)
        return max_len
```

## Python Set Data Structure

### What is a Set?

A set is an unordered collection of unique elements. Key properties:

```python
# Creating a set
my_set = set()
my_set = {'a', 'b', 'c'}

# No duplicates allowed
my_set = {'a', 'b', 'a'}  # Result: {'a', 'b'}
```

### Set Operations Used in Our Solution

```python
# 1. Check if element exists - O(1)
if 'a' in window:  # Very fast!
    print("Found")

# 2. Add element - O(1)
window.add('a')

# 3. Remove element - O(1)
window.remove('a')

# 4. Size of set
len(window)
```

### Why Set Instead of List?

```python
# List - SLOW
my_list = ['a', 'b', 'c']
if 'd' in my_list:  # O(n) - checks each element
    pass

# Set - FAST
my_set = {'a', 'b', 'c'}
if 'd' in my_set:  # O(1) - instant lookup
    pass
```

### Set vs Dictionary

```python
# Set - Only stores values
window = {'a', 'b', 'c'}

# Dictionary - Stores key-value pairs
char_index = {'a': 0, 'b': 1, 'c': 2}
```

## Solution Approach - Sliding Window

### Core Idea

Use two pointers (left and right) to create a "window" and slide it through the string to find the longest substring without repeating characters.

## Step-by-Step Walkthrough

Let's trace through `s = "abcabcbb"`:

### Initial State
```
window = set()
max_len = 0
left = 0
```

### Step 1: right=0, char='a'
```
"a b c a b c b b"
 ↑
 LR
```
- 'a' not in window
- window = {'a'}
- max_len = 1

### Step 2: right=1, char='b'
```
"a b c a b c b b"
 ↑ ↑
 L R
```
- 'b' not in window
- window = {'a', 'b'}
- max_len = 2

### Step 3: right=2, char='c'
```
"a b c a b c b b"
 ↑   ↑
 L   R
```
- 'c' not in window
- window = {'a', 'b', 'c'}
- max_len = 3

### Step 4: right=3, char='a'
```
"a b c a b c b b"
 ↑     ↑
 L     R
```
- 'a' IS in window! (duplicate found)
- While loop: remove 'a', left++
- window = {'b', 'c'}
- left = 1

```
"a b c a b c b b"
   ↑   ↑
   L   R
```
- Now add 'a'
- window = {'b', 'c', 'a'}
- max_len = 3 (unchanged)

## Code Explanation

```python
def lengthOfLongestSubstring(self, s: str) -> int:
    window = set()  # Characters in current window (no duplicates)
    max_len = 0     # Maximum substring length found
    left = 0        # Left pointer of window
    
    for right, char in enumerate(s):  # Right pointer
        # If duplicate found, shrink window from left
        while char in window:
            window.remove(s[left])  # Remove leftmost character
            left += 1               # Move left pointer right
            
        # Add current character
        window.add(char)
        
        # Update maximum length
        # Window size = right - left + 1
        max_len = max(max_len, right - left + 1)
    
    return max_len
```

## Why While Loop Instead of If?

```python
# Example: "abba" at right=2 (second 'b')
# window = {'a', 'b'}, left=0

while 'b' in window:  # True
    window.remove('a')  # Remove s[0] = 'a'
    left = 1
    # window = {'b'}
    
# Still 'b' in window? Yes!
while 'b' in window:  # True
    window.remove('b')  # Remove s[1] = 'b'
    left = 2
    # window = {}
```

The while loop ensures we remove ALL characters until no duplicate exists.

## Complexity Analysis

### Time Complexity: O(n)

```python
for right in range(n):      # n iterations
    while char in window:   # Each character added once, removed once
        window.remove()     # O(1) - set operation
        left += 1
    window.add()           # O(1) - set operation
```

- Each character is visited at most twice (once by right pointer, once by left)
- Total: O(2n) = O(n)

### Space Complexity: O(min(n, m))

- n = length of string
- m = size of alphabet (e.g., 26 for lowercase letters, 128 for ASCII)
- Set can contain at most m different characters

## Visual Representation

```
Sliding Window Technique:

"a b c a b c b b"
 └─┘              window = "ab", len = 2
   └─┘            window = "bc", len = 2  
     └─┘          window = "ca", len = 2
       └───┘      window = "abc", len = 3 ← MAX!
```

## Alternative Solutions Comparison

### 1. Brute Force
```python
def lengthOfLongestSubstring_brute(s):
    max_len = 0
    for i in range(len(s)):
        for j in range(i+1, len(s)+1):
            substring = s[i:j]
            if len(substring) == len(set(substring)):
                max_len = max(max_len, len(substring))
    return max_len
```
- **Time:** O(n³)
- **Space:** O(n)

### 2. HashMap with Index Tracking
```python
def lengthOfLongestSubstring_hashmap(s):
    char_index = {}
    max_len = 0
    left = 0
    
    for right, char in enumerate(s):
        if char in char_index and char_index[char] >= left:
            left = char_index[char] + 1
        char_index[char] = right
        max_len = max(max_len, right - left + 1)
    
    return max_len
```
- **Time:** O(n)
- **Space:** O(min(n, m))
- **Advantage:** No while loop needed

## Comparison Table

| Solution | Time Complexity | Space Complexity | Description |
|----------|----------------|------------------|-------------|
| **Sliding Window (Set)** | O(n) | O(min(n,m)) | Two pointers with set |
| **Brute Force** | O(n³) | O(n) | Check all substrings |
| **HashMap with Index** | O(n) | O(min(n,m)) | Track character positions |

## Key Insights

1. **Set for Uniqueness:** Perfect for tracking unique characters
2. **Two Pointers:** Efficiently manage the window boundaries
3. **While Loop:** Ensures all duplicates are removed before proceeding
4. **Optimal Solution:** Both time and space efficient

## Common Mistakes

1. Using `if` instead of `while` (doesn't handle all duplicates)
2. Forgetting to update `max_len` at each step
3. Incorrect window size calculation
4. Using list instead of set (O(n) lookup vs O(1))

## Related Problems

- Minimum Window Substring
- Longest Substring with At Most K Distinct Characters
- Permutation in String
- Find All Anagrams in a String

---

*This solution demonstrates the power of the sliding window technique combined with efficient data structures (set) to achieve optimal O(n) time complexity.*