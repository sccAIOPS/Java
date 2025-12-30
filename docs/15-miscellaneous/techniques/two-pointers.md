# Two Pointers Technique

> **Category:** Miscellaneous Algorithms  
> **Subcategory:** Array Techniques  
> **Implementation:** [`TwoPointers.java`](../../src/main/java/com/thealgorithms/others/TwoPointers.java)

---

## 📚 Overview

The Two Pointers technique is an algorithmic pattern that uses two pointers (or indices) to traverse data structures, typically arrays or linked lists. It reduces time complexity from O(n²) to O(n) for many problems by eliminating redundant iterations.

**Key Characteristics:**
- Reduces nested loops to single traversal
- Often used with sorted arrays
- Space efficient O(1)
- Applicable to various problems

---

## 🔢 Types of Two Pointers

### 1. Opposite Direction (Two Ends)
Pointers start at opposite ends and move toward each other.
```
[1, 2, 3, 4, 5]
 ↑           ↑
left       right
```

### 2. Same Direction (Fast/Slow)
Both pointers start from same position, moving at different speeds.
```
[1, 2, 3, 4, 5]
 ↑  ↑
slow fast
```

### 3. Sliding Window (Fixed/Variable)
Pointers define a window that slides through the array.
```
[1, 2, 3, 4, 5]
 ↑     ↑
start end
```

---

## 📊 Complexity Analysis

| Without Two Pointers | With Two Pointers |
|---------------------|-------------------|
| O(n²) or O(n³) | O(n) or O(n log n) |

Space: O(1) in most cases.

---

## 🔄 Algorithms (Pseudocode)

### Problem 1: Two Sum (Sorted Array)
```
ALGORITHM TwoSumSorted(arr, target)
─────────────────────────────────────────────────────
    left ← 0
    right ← length(arr) - 1
    
    WHILE left < right DO
        sum ← arr[left] + arr[right]
        
        IF sum = target THEN
            RETURN (left, right)
        ELSE IF sum < target THEN
            left ← left + 1
        ELSE
            right ← right - 1
        END IF
    END WHILE
    
    RETURN NOT_FOUND
```

### Problem 2: Remove Duplicates
```
ALGORITHM RemoveDuplicates(arr)
─────────────────────────────────────────────────────
    IF length(arr) = 0 THEN RETURN 0
    
    slow ← 0
    
    FOR fast ← 1 TO length(arr) - 1 DO
        IF arr[fast] ≠ arr[slow] THEN
            slow ← slow + 1
            arr[slow] ← arr[fast]
        END IF
    END FOR
    
    RETURN slow + 1  // New length
```

### Problem 3: Container With Most Water
```
ALGORITHM MaxArea(heights)
─────────────────────────────────────────────────────
    left ← 0
    right ← length(heights) - 1
    maxArea ← 0
    
    WHILE left < right DO
        width ← right - left
        height ← min(heights[left], heights[right])
        area ← width × height
        maxArea ← max(maxArea, area)
        
        IF heights[left] < heights[right] THEN
            left ← left + 1
        ELSE
            right ← right - 1
        END IF
    END WHILE
    
    RETURN maxArea
```

### Problem 4: Palindrome Check
```
ALGORITHM IsPalindrome(s)
─────────────────────────────────────────────────────
    left ← 0
    right ← length(s) - 1
    
    WHILE left < right DO
        IF s[left] ≠ s[right] THEN
            RETURN FALSE
        END IF
        left ← left + 1
        right ← right - 1
    END WHILE
    
    RETURN TRUE
```

---

## 💻 Implementation Notes

### Java Implementation

```java
public class TwoPointers {
    
    // Two Sum in sorted array
    public static int[] twoSum(int[] arr, int target) {
        int left = 0;
        int right = arr.length - 1;
        
        while (left < right) {
            int sum = arr[left] + arr[right];
            
            if (sum == target) {
                return new int[]{left, right};
            } else if (sum < target) {
                left++;
            } else {
                right--;
            }
        }
        
        return new int[]{-1, -1};  // Not found
    }
    
    // Remove duplicates in-place
    public static int removeDuplicates(int[] arr) {
        if (arr.length == 0) return 0;
        
        int slow = 0;
        
        for (int fast = 1; fast < arr.length; fast++) {
            if (arr[fast] != arr[slow]) {
                slow++;
                arr[slow] = arr[fast];
            }
        }
        
        return slow + 1;
    }
    
    // Container with most water
    public static int maxArea(int[] heights) {
        int left = 0;
        int right = heights.length - 1;
        int maxArea = 0;
        
        while (left < right) {
            int width = right - left;
            int height = Math.min(heights[left], heights[right]);
            maxArea = Math.max(maxArea, width * height);
            
            if (heights[left] < heights[right]) {
                left++;
            } else {
                right--;
            }
        }
        
        return maxArea;
    }
    
    // Three Sum
    public static List<List<Integer>> threeSum(int[] nums, int target) {
        Arrays.sort(nums);
        List<List<Integer>> result = new ArrayList<>();
        
        for (int i = 0; i < nums.length - 2; i++) {
            if (i > 0 && nums[i] == nums[i - 1]) continue;
            
            int left = i + 1;
            int right = nums.length - 1;
            
            while (left < right) {
                int sum = nums[i] + nums[left] + nums[right];
                
                if (sum == target) {
                    result.add(Arrays.asList(nums[i], nums[left], nums[right]));
                    while (left < right && nums[left] == nums[left + 1]) left++;
                    while (left < right && nums[right] == nums[right - 1]) right--;
                    left++;
                    right--;
                } else if (sum < target) {
                    left++;
                } else {
                    right--;
                }
            }
        }
        
        return result;
    }
}
```

### Code Reference

📁 **Source File:** [`src/main/java/com/thealgorithms/others/TwoPointers.java`](../../src/main/java/com/thealgorithms/others/TwoPointers.java)

---

## 🌍 Common Problems

| Problem | Technique | Time |
|---------|-----------|------|
| Two Sum (sorted) | Opposite | O(n) |
| Remove Duplicates | Same direction | O(n) |
| Palindrome | Opposite | O(n) |
| Three Sum | Opposite + loop | O(n²) |
| Merge Sorted Arrays | Same direction | O(n+m) |
| Linked List Cycle | Fast/Slow | O(n) |
| Container With Most Water | Opposite | O(n) |

---

## ⚖️ When to Use Two Pointers

### Good Candidates
- ✅ Sorted arrays
- ✅ Finding pairs/triplets
- ✅ In-place operations
- ✅ Palindrome problems
- ✅ Merging/partitioning

### Not Suitable
- ❌ Unsorted data (unless can be sorted)
- ❌ Non-linear data structures
- ❌ Problems requiring all pairs

---

## ⚠️ Common Pitfalls

| Pitfall | Problem | Solution |
|---------|---------|----------|
| Array not sorted | Wrong results | Sort first or use hash |
| Infinite loop | Pointers don't move | Ensure termination |
| Off-by-one | Wrong bounds | Test edge cases |
| Duplicate handling | Missing or extra results | Skip duplicates |

---

## 📖 References

1. **"Competitive Programming 3"** - Halim & Halim
2. **"Algorithms"** - Sedgewick & Wayne
3. **LeetCode Two Pointers Problems**

---

## 🔗 Related Algorithms

- [Sliding Window](../../slidingwindow/README.md)
- [Binary Search](../../02-searching-algorithms/binary-search.md)
- [Merge Sort](../../01-sorting-algorithms/comparison-based/mergesort.md)

---

*Last updated: December 30, 2025*
