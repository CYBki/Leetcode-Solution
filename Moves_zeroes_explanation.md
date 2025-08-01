# Two Pointers: Shift Zeros to the End - Algorithm Explanation

## Problem Description

This algorithm shifts all zeros in an array to the end while maintaining the relative order of non-zero elements. The operation is performed **in-place**, meaning no additional memory is used.

## Algorithm Logic

### Two Pointers Approach

The algorithm uses two pointers:
- **left**: Points to the position where non-zero elements should be placed
- **right**: Traverses through the array and checks all elements

### Working Principle

```python
def shift_zeros_to_the_end(nums: List[int]) -> None:
    # The 'left' pointer is used to position non-zero elements
    left = 0
    
    # Iterate through the array using a 'right' pointer to locate non-zero elements
    for right in range(len(nums)):
        if nums[right] != 0:
            # Non-zero element found, move it to the left position
            nums[left], nums[right] = nums[right], nums[left]
            # Increment 'left' since it now points to a position already occupied
            # by a non-zero element
            left += 1
```

### Step-by-Step Execution

1. **Initialization**: `left = 0` is set
2. **Iteration**: `right` pointer checks every element in the array
3. **Non-Zero Element Found**: 
   - If `nums[right] != 0`
   - Swap `nums[left]` with `nums[right]`
   - Increment `left`
4. **Continue**: `right` moves to the next element

### Example Walkthrough

**Input**: `[0, 1, 0, 3, 12]`

| Step | right | nums[right] | left | Action | Array State |
|------|-------|-------------|------|--------|-------------|
| 0    | 0     | 0           | 0    | -      | [0, 1, 0, 3, 12] |
| 1    | 1     | 1           | 0    | Swap   | [1, 0, 0, 3, 12] |
| 2    | 2     | 0           | 1    | -      | [1, 0, 0, 3, 12] |
| 3    | 3     | 3           | 1    | Swap   | [1, 3, 0, 0, 12] |
| 4    | 4     | 12          | 2    | Swap   | [1, 3, 12, 0, 0] |

**Output**: `[1, 3, 12, 0, 0]`

## Complexity Analysis

### Time Complexity: O(n)
- If the array size is `n`, each element is visited only once
- Each operation takes constant time O(1)

### Space Complexity: O(1)
- Only two pointers (`left` and `right`) are used
- No additional memory space required (in-place operation)

## Test Scenarios

| Input | Expected Output | Description |
|-------|----------------|-------------|
| `[]` | `[]` | Empty array test |
| `[0]` | `[0]` | Single zero element |
| `[1]` | `[1]` | Single non-zero element |
| `[0, 0, 0]` | `[0, 0, 0]` | All elements are zeros |
| `[1, 3, 2]` | `[1, 3, 2]` | No zeros present |
| `[1, 1, 1, 0, 0]` | `[1, 1, 1, 0, 0]` | Zeros already at the end |
| `[0, 0, 1, 1, 1]` | `[1, 1, 1, 0, 0]` | Zeros at the beginning |

## Algorithm Advantages

1. **Efficiency**: Optimal with O(n) time complexity
2. **Memory Friendly**: O(1) space complexity
3. **Stability**: Relative order of non-zero elements is preserved
4. **Simplicity**: Easy to understand and implement

## Key Observations

- The `right` pointer always moves forward (regardless of zero or non-zero)
- This allows us to use a `for` loop for iteration
- The `left` pointer only advances when a non-zero element is found
- The swap operation is always safe (even if both pointers point to the same position)

## Why This Approach Works

The algorithm works by maintaining an invariant: all elements from index `0` to `left-1` are non-zero elements in their correct relative positions. When we find a non-zero element at position `right`, we place it at position `left` and increment `left`. This ensures that:

1. Non-zero elements are moved to the front
2. Their relative order is maintained
3. Zeros naturally end up at the back
4. The operation is done in-place with optimal time complexity