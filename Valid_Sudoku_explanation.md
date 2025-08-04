# Valid Sudoku - Problem Analysis and Solution

## Problem Description

**LeetCode 36 - Valid Sudoku**

Determine if a 9x9 Sudoku board is valid. Only the filled cells need to be validated according to the following rules:

1. Each row must contain the digits 1-9 without repetition
2. Each column must contain the digits 1-9 without repetition  
3. Each of the nine 3x3 sub-boxes must contain the digits 1-9 without repetition

**Important Notes:**
- A Sudoku board (partially filled) could be valid but is not necessarily solvable
- Only filled cells need to be validated according to the mentioned rules
- Empty cells are represented by the character `'.'`

## Example

```
Input: board = 
[["5","3",".",".","7",".",".",".","."]
,["6",".",".","1","9","5",".",".","."]
,[".","9","8",".",".",".",".","6","."]
,["8",".",".",".","6",".",".",".","3"]
,["4",".",".","8",".","3",".",".","1"]
,["7",".",".",".","2",".",".",".","6"]
,[".","6",".",".",".",".","2","8","."]
,[".",".",".","4","1","9",".",".","5"]
,[".",".",".",".","8",".",".","7","9"]]

Output: true
```

## Solution Approach

### Algorithm Strategy

1. **Set-based Tracking**: Use separate sets to track seen numbers for:
   - Each of the 9 rows
   - Each of the 9 columns
   - Each of the 9 sub-grids (3x3 boxes)

2. **Single Pass**: Iterate through the entire board once and validate all three constraints simultaneously

3. **Sub-grid Indexing**: Calculate sub-grid position using integer division: `(row // 3, col // 3)`

### Implementation

```python
class Solution:
    def isValidSudoku(self, board: List[List[str]]) -> bool:
        row_sets = [set() for _ in range(9)]
        column_sets = [set() for _ in range(9)]
        subgrid_sets = [[set() for _ in range(3)] for _ in range(3)]
        
        for r in range(9):
            for c in range(9):
                num = board[r][c]
                if num == ".":  # Skip empty cells
                    continue
                
                # Check for duplicates
                if num in row_sets[r]:
                    return False
                if num in column_sets[c]:
                    return False
                if num in subgrid_sets[r // 3][c // 3]:
                    return False

                # Add number to respective sets
                row_sets[r].add(num)
                column_sets[c].add(num)
                subgrid_sets[r // 3][c // 3].add(num)
        
        return True
```

## Complexity Analysis

### Time Complexity: **O(1)**
- We iterate through a fixed 9×9 board (81 cells)
- Each cell operation (set lookup and insertion) is O(1)
- Total operations: 81 × O(1) = O(1)
- **Reasoning**: Since the board size is constant (not dependent on input size n), the time complexity is constant

### Space Complexity: **O(1)**
- **Row sets**: 9 sets, each storing at most 9 unique digits → 9 × 9 = 81 elements max
- **Column sets**: 9 sets, each storing at most 9 unique digits → 9 × 9 = 81 elements max  
- **Sub-grid sets**: 3×3 = 9 sets, each storing at most 9 unique digits → 9 × 9 = 81 elements max
- **Total space**: 81 + 81 + 81 = 243 elements maximum
- **Reasoning**: Since the maximum space usage is constant (not dependent on input size), space complexity is O(1)

## Key Implementation Details

### 1. Sub-grid Calculation
```python
subgrid_row = r // 3  # Maps rows 0-2→0, 3-5→1, 6-8→2
subgrid_col = c // 3  # Maps cols 0-2→0, 3-5→1, 6-8→2
```

### 2. Early Termination
- Return `False` immediately when a duplicate is found
- No need to continue checking once invalidity is detected

### 3. Empty Cell Handling
- Skip processing for cells containing `"."` 
- Only validate filled cells as per problem requirements

## Algorithm Visualization

```
Board positions mapping to sub-grids:
┌─────────┬─────────┬─────────┐
│ (0,0)   │ (0,1)   │ (0,2)   │
│ 0 1 2   │ 0 1 2   │ 0 1 2   │
├─────────┼─────────┼─────────┤
│ (1,0)   │ (1,1)   │ (1,2)   │
│ 3 4 5   │ 3 4 5   │ 3 4 5   │
├─────────┼─────────┼─────────┤
│ (2,0)   │ (2,1)   │ (2,2)   │
│ 6 7 8   │ 6 7 8   │ 6 7 8   │
└─────────┴─────────┴─────────┘
```

## Edge Cases Handled

1. **Empty board**: All cells are `"."` → Valid (returns `true`)
2. **Fully filled valid board**: All constraints satisfied → Valid
3. **Partially filled with conflicts**: Any duplicate found → Invalid
4. **Single invalid cell**: Early termination prevents unnecessary computation

## Alternative Approaches

### Approach 1: Multiple Passes (Less Efficient)
- First pass: Check all rows
- Second pass: Check all columns  
- Third pass: Check all sub-grids
- **Time**: Still O(1) but with higher constant factor
- **Space**: O(1)

### Approach 2: Hash Map with Encoding
```python
seen = set()
for r in range(9):
    for c in range(9):
        if board[r][c] != '.':
            num = board[r][c]
            if (f"row{r}-{num}" in seen or 
                f"col{c}-{num}" in seen or 
                f"box{r//3}{c//3}-{num}" in seen):
                return False
            seen.add(f"row{r}-{num}")
            seen.add(f"col{c}-{num}")  
            seen.add(f"box{r//3}{c//3}-{num}")
```
- **Time**: O(1)
- **Space**: O(1) 
- **Trade-off**: More memory due to string encoding but single set to manage

## Conclusion

The implemented solution efficiently validates a Sudoku board in a single pass with optimal time and space complexity. The use of separate sets for tracking constraints provides clear, readable code while maintaining excellent performance characteristics.