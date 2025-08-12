# Integer to Roman - LeetCode Problem Solution

## Problem Description

Given an integer, convert it to a roman numeral.

**Constraints:**
- `1 <= num <= 3999`

### Roman Numeral Rules

Roman numerals are represented by seven different symbols:

| Symbol | Value |
|--------|-------|
| I      | 1     |
| V      | 5     |
| X      | 10    |
| L      | 50    |
| C      | 100   |
| D      | 500   |
| M      | 1000  |

**Special Cases (Subtraction Rule):**
- I can be placed before V (5) and X (10) to make 4 and 9
- X can be placed before L (50) and C (100) to make 40 and 90
- C can be placed before D (500) and M (1000) to make 400 and 900

### Examples

| Input | Output |
|-------|--------|
| 3     | "III"  |
| 58    | "LVIII" |
| 1994  | "MCMXCIV" |

## Solution Approach

### Algorithm: Greedy Method

The key insight is to use a **greedy approach** - always use the largest possible Roman numeral value first.

**Strategy:**
1. Create a mapping of values to Roman numerals in descending order
2. Include subtraction cases (4, 9, 40, 90, 400, 900) as separate entries
3. For each value-symbol pair, determine how many times it fits into the number
4. Append the corresponding symbols and subtract the used value
5. Continue until the number becomes 0

### Why Greedy Works

Roman numerals follow a specific structure where larger values come before smaller ones. The subtraction rule only applies in specific cases (4, 9, 40, 90, 400, 900), and by including these as separate mappings, we ensure the greedy approach always produces the correct result.

## Implementation

### Solution 1: Tuple-Based Approach

```python
class Solution:
    def intToRoman(self, num: int) -> str:
        # Value-symbol pairs in descending order
        # Including subtraction cases as separate entries
        value_symbols = [
            (1000, 'M'), (900, 'CM'), (500, 'D'), (400, 'CD'),
            (100, 'C'), (90, 'XC'), (50, 'L'), (40, 'XL'),
            (10, 'X'), (9, 'IX'), (5, 'V'), (4, 'IV'), (1, 'I')
        ]
        
        res = []
        
        for value, symbol in value_symbols:
            if num == 0:
                break
            
            # Calculate how many times this value fits
            count = num // value
            
            # Append the symbol 'count' times
            res.append(symbol * count)
            
            # Subtract the used value
            num -= count * value
        
        return ''.join(res)
```

### Solution 2: While Loop Approach

```python
class Solution:
    def intToRoman(self, num: int) -> str:
        values = [1000, 900, 500, 400, 100, 90, 50, 40, 10, 9, 5, 4, 1]
        symbols = ["M", "CM", "D", "CD", "C", "XC", "L", "XL", "X", "IX", "V", "IV", "I"]
        
        ans = ""
        
        for i in range(len(values)):
            while num >= values[i]:
                ans += symbols[i]
                num -= values[i]
        
        return ans
```

## Step-by-Step Walkthrough

Let's trace through both solutions using the example `num = 1994`:

### Solution 1 (Tuple-based): 

| Step | Value | Symbol | Count | Action | Remaining | Result So Far |
|------|-------|--------|-------|---------|-----------|---------------|
| 1    | 1000  | 'M'    | 1     | Add 'M' | 994       | ['M']         |
| 2    | 900   | 'CM'   | 1     | Add 'CM'| 94        | ['M', 'CM']   |
| 3    | 500   | 'D'    | 0     | Skip    | 94        | ['M', 'CM']   |
| 4    | 400   | 'CD'   | 0     | Skip    | 94        | ['M', 'CM']   |
| 5    | 100   | 'C'    | 0     | Skip    | 94        | ['M', 'CM']   |
| 6    | 90    | 'XC'   | 1     | Add 'XC'| 4         | ['M', 'CM', 'XC'] |
| 7    | 50    | 'L'    | 0     | Skip    | 4         | ['M', 'CM', 'XC'] |
| 8    | 40    | 'XL'   | 0     | Skip    | 4         | ['M', 'CM', 'XC'] |
| 9    | 10    | 'X'    | 0     | Skip    | 4         | ['M', 'CM', 'XC'] |
| 10   | 9     | 'IX'   | 0     | Skip    | 4         | ['M', 'CM', 'XC'] |
| 11   | 5     | 'V'    | 0     | Skip    | 4         | ['M', 'CM', 'XC'] |
| 12   | 4     | 'IV'   | 1     | Add 'IV'| 0         | ['M', 'CM', 'XC', 'IV'] |

**Final Result:** `''.join(['M', 'CM', 'XC', 'IV']) = "MCMXCIV"`

### Solution 2 (While loop): 

| Step | Index | Value | Symbol | Action | Remaining | Result So Far |
|------|-------|-------|--------|--------|-----------|---------------|
| 1    | 0     | 1000  | 'M'    | Add 'M' | 994      | "M"           |
| 2    | 1     | 900   | 'CM'   | Add 'CM'| 94       | "MCM"         |
| 3    | 2-4   | 500-100| Skip   | Skip   | 94       | "MCM"         |
| 4    | 5     | 90    | 'XC'   | Add 'XC'| 4        | "MCMXC"       |
| 5    | 6-11  | 50-5  | Skip   | Skip   | 4        | "MCMXC"       |
| 6    | 12    | 4     | 'IV'   | Add 'IV'| 0        | "MCMXCIV"     |

**Final Result:** `"MCMXCIV"`

## Comparison of Solutions

| Aspect | Solution 1 (Tuple) | Solution 2 (While Loop) |
|--------|-------------------|-------------------------|
| **Readability** | More explicit with tuples | Cleaner, more straightforward |
| **String Operations** | Uses list + join | Direct string concatenation |
| **Memory Usage** | Temporary list storage | Direct string building |
| **Performance** | Slightly better (fewer string ops) | Slightly slower (more string ops) |
| **Code Length** | More verbose | More concise |
| **Maintainability** | Data structure is clear | Logic is more intuitive |

## Complexity Analysis

### Time Complexity: O(1)
- **Constant Time**: The algorithm always iterates through the same 13 value-symbol pairs regardless of input size
- Each iteration performs constant time operations (division, multiplication, subtraction)
- Since the number of iterations is fixed and small (13), the time complexity is O(1)

### Space Complexity: O(1)
- **Constant Space**: The space used doesn't grow with input size
- The `value_symbols` list always contains 13 pairs
- The `res` list stores at most 13 elements (in practice, much fewer)
- The maximum length of a Roman numeral for numbers ≤ 3999 is bounded

### Detailed Complexity Explanation

**Why O(1) and not O(log n)?**

While it might seem like the complexity should be O(log n) because we're repeatedly dividing the number, the key insight is that:

1. **Fixed number of operations**: We always check exactly 13 value-symbol pairs
2. **Bounded input**: The constraint `1 ≤ num ≤ 3999` means we're working with a fixed range
3. **Bounded output**: The longest Roman numeral in this range has a fixed maximum length

For example, 3999 = "MMMCMXCIX" (9 characters), which is the longest possible output.

## Alternative Approaches

### 1. Hardcoded Arrays (Most Optimized)
```python
def intToRoman(self, num: int) -> str:
    thousands = ["", "M", "MM", "MMM"]
    hundreds = ["", "C", "CC", "CCC", "CD", "D", "DC", "DCC", "DCCC", "CM"]
    tens = ["", "X", "XX", "XXX", "XL", "L", "LX", "LXX", "LXXX", "XC"]
    ones = ["", "I", "II", "III", "IV", "V", "VI", "VII", "VIII", "IX"]
    
    return (thousands[num // 1000] + 
            hundreds[(num % 1000) // 100] + 
            tens[(num % 100) // 10] + 
            ones[num % 10])
```

**Pros:** Slightly faster, direct lookup
**Cons:** More memory usage, harder to understand

### 2. Recursive Approach
```python
def intToRoman(self, num: int) -> str:
    values = [1000, 900, 500, 400, 100, 90, 50, 40, 10, 9, 5, 4, 1]
    symbols = ["M", "CM", "D", "CD", "C", "XC", "L", "XL", "X", "IX", "V", "IV", "I"]
    
    for i, value in enumerate(values):
        if num >= value:
            return symbols[i] + self.intToRoman(num - value)
    return ""
```

**Pros:** Elegant recursive structure
**Cons:** Function call overhead, slightly less efficient

## Key Takeaways

1. **Greedy approach works** because of Roman numeral structure
2. **Include subtraction cases** as separate mappings to handle them naturally
3. **Time and space complexity are O(1)** due to bounded input and fixed operations
4. **Order matters** - process from largest to smallest values
5. **Simple and efficient** - the straightforward approach is often the best

## Test Cases

```python
# Test the solution
solution = Solution()

test_cases = [
    (3, "III"),
    (4, "IV"),
    (9, "IX"),
    (58, "LVIII"),
    (1994, "MCMXCIV"),
    (3999, "MMMCMXCIX")
]

for num, expected in test_cases:
    result = solution.intToRoman(num)
    print(f"Input: {num}, Output: {result}, Expected: {expected}, ✓" if result == expected else "✗")
```

This solution efficiently converts integers to Roman numerals using a greedy approach with optimal time and space complexity.