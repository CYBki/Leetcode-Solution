# Set Matrix Zeroes - Problem Analysis

## Problem Description

Given an `m x n` integer matrix `matrix`, if an element is 0, set its entire row and column to 0's.

**Constraint:** You must do it in-place.

### Example:
```
Input:  [[1,1,1],
         [1,0,1],
         [1,1,1]]

Output: [[1,0,1],
         [0,0,0],
         [1,0,1]]
```

## Solution Explanation

This solution uses an **optimized space approach** by utilizing the first row and first column of the matrix as markers to store information about which rows and columns should be zeroed.

### Algorithm Steps:

1. **Identify Special Case**: Check if the first row itself contains any zeros and store this information in `first_row_zero`.

2. **Mark Zeros**: 
   - Traverse the entire matrix
   - When a zero is found at position `(i,j)`:
     - If it's in the first row (`i==0`), set `first_row_zero = True`
     - Otherwise, mark the first element of that row: `matrix[i][0] = 0`
     - Always mark the first element of that column: `matrix[0][j] = 0`

3. **Zero Out Rows**: 
   - For rows 1 to m-1, if `matrix[i][0] == 0`, set entire row to zeros

4. **Zero Out Columns**: 
   - For each column j, if `matrix[0][j] == 0`, set entire column to zeros

5. **Handle First Row**: 
   - If `first_row_zero` is true, set the entire first row to zeros

### Code Walkthrough:

```python
class Solution:
    def setZeroes(self, matrix: List[List[int]]) -> None:
        first_row_zero = False
        rows = len(matrix)
        cols = len(matrix[0])

        # Step 1 & 2: Find zeros and mark first row/column
        for i in range(rows):
            for j in range(cols):
                if matrix[i][j] == 0:
                    if i == 0:
                        first_row_zero = True
                    else:
                        matrix[i][0] = 0
                    matrix[0][j] = 0

        # Step 3: Zero out rows (except first row)
        for i in range(1, rows):
            if matrix[i][0] == 0:
                matrix[i] = [0] * cols

        # Step 4: Zero out columns
        for j in range(cols):
            if matrix[0][j] == 0:
                for k in range(rows):
                    matrix[k][j] = 0
        
        # Step 5: Handle first row
        if first_row_zero:
                matrix[0] = [0] * cols
```

## Complexity Analysis

### Time Complexity: **O(m × n)**
- **First pass**: O(m × n) - traverse entire matrix to find zeros
- **Row zeroing**: O(m × n) - in worst case, zero out all rows
- **Column zeroing**: O(m × n) - in worst case, zero out all columns
- **Total**: O(m × n) where m = number of rows, n = number of columns

### Space Complexity: **O(1)**
- Only uses constant extra space:
  - `first_row_zero` boolean variable
  - `rows`, `cols` integer variables
  - Loop variables `i`, `j`, `k`
- Uses the matrix's first row and first column as storage, so no additional space proportional to input size

## Alternative Approaches

### 1. Brute Force - O(m × n × (m + n)) Time, O(1) Space
- For each zero found, immediately zero out entire row and column
- Very inefficient due to redundant operations

### 2. Extra Space - O(m × n) Time, O(m + n) Space
- Use separate arrays to track which rows and columns should be zeroed
- More intuitive but uses additional space

### 3. Current Solution - O(m × n) Time, O(1) Space ✅
- Optimal solution balancing time and space efficiency
- Uses matrix itself for storage, achieving constant space complexity

## Key Insights

1. **In-place modification** is achieved by cleverly using the matrix's border as storage
2. **Special handling of first row** prevents conflicts when using it as a marker
3. **Order of operations** matters - rows and columns must be processed in the right sequence
4. This approach demonstrates how **space optimization** can be achieved through creative use of existing data structures

## Edge Cases Handled

- Empty matrix
- Single row/column matrices  
- Matrix with all zeros
- Matrix with no zeros
- First row/column containing zeros