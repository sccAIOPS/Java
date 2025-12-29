# RadixSort

> **Category:** Non-Comparison Sorting  
> **Paradigm:** Distribution / Digit-by-Digit  
> **In-Place:** No  
> **Stable:** Yes  
> **Input Restriction:** Non-negative integers (this implementation)

---

## 📋 Overview

RadixSort sorts integers by processing individual digits. It processes digits from least significant digit (LSD) to most significant digit (MSD), using a stable sort (typically CountingSort) as a subroutine for each digit. RadixSort achieves **linear time complexity** when the number of digits is constant.

### Key Characteristics

| Property | Value |
|----------|-------|
| **Time (Best)** | O(d × (n + k)) |
| **Time (Average)** | O(d × (n + k)) |
| **Time (Worst)** | O(d × (n + k)) |
| **Space** | O(n + k) |
| **Stable** | Yes |
| **In-Place** | No |

Where:
- $d$ = number of digits in the maximum number
- $n$ = number of elements
- $k$ = base (radix), typically 10

---

## 🔬 Mathematical Analysis

### Algorithm Concept

RadixSort processes digits from **right to left** (LSD first):
1. Sort all numbers by the least significant digit
2. Sort by the next digit (maintaining stability)
3. Repeat until most significant digit

### Why LSD First? (Stability is Key!)

Consider sorting `[170, 45, 75, 90, 802, 24, 2, 66]`:

If we sorted MSD first (unstable), we'd lose the relative order of numbers with the same leading digit.

With LSD and stable sort:
- After sorting by units: [170, 90, 802, 2, 24, 45, 75, 66]
- After sorting by tens: [802, 2, 24, 45, 66, 170, 75, 90]
- After sorting by hundreds: [2, 24, 45, 66, 75, 90, 170, 802] ✅

### Time Complexity Derivation

| Step | Operations |
|------|------------|
| Find max | O(n) |
| For each digit (d times) | |
| → CountingSort | O(n + k) |

**Total:**
$$
T(n) = O(n) + d \cdot O(n + k) = O(d \cdot (n + k))
$$

For fixed base $k = 10$ and $d = \log_{10}(\max)$:
$$
T(n) = O(n \cdot \log_{10}(\max))
$$

### When is RadixSort Efficient?

For 32-bit integers with base 10:
- $d = 10$ digits maximum
- $T(n) = O(10 \cdot (n + 10)) = O(n)$ **linear!**

For base 256 (byte-by-byte):
- $d = 4$ passes for 32-bit integers
- Very efficient in practice

---

## 📝 Pseudocode

```
RADIX-SORT(A)
    if A is empty then
        return A
    
    // Validate: no negative numbers
    for each number in A do
        if number < 0 then
            throw IllegalArgumentException
    
    max ← MAX(A)
    d ← number of digits in max
    
    // Process each digit from LSD to MSD
    exp ← 1
    for i ← 0 to d-1 do
        COUNTING-SORT-BY-DIGIT(A, exp)
        exp ← exp × BASE
    
    return A

COUNTING-SORT-BY-DIGIT(A, exp)
    // Count occurrences of each digit
    count ← array[0..BASE-1] = 0
    for each number in A do
        digit ← (number / exp) mod BASE
        count[digit]++
    
    // Cumulative count
    for i ← 1 to BASE-1 do
        count[i] ← count[i] + count[i-1]
    
    // Build output (backwards for stability)
    output ← array of size n
    for i ← n-1 downto 0 do
        digit ← (A[i] / exp) mod BASE
        output[count[digit] - 1] ← A[i]
        count[digit]--
    
    // Copy back
    A ← output
```

---

## 💻 Implementation

### Source File

**Location:** [src/main/java/com/thealgorithms/sorts/RadixSort.java](../../src/main/java/com/thealgorithms/sorts/RadixSort.java)

```java
/**
 * This class provides an implementation of the radix sort algorithm.
 * It sorts an array of nonnegative integers in increasing order.
 */
public final class RadixSort {
    private static final int BASE = 10;

    private RadixSort() {
    }

    /**
     * Sorts an array of nonnegative integers using the radix sort algorithm.
     *
     * @param array the array to be sorted
     * @return the sorted array
     * @throws IllegalArgumentException if any negative integers are found
     */
    public static int[] sort(int[] array) {
        if (array.length == 0) {
            return array;
        }

        checkForNegativeInput(array);
        radixSort(array);
        return array;
    }

    private static void checkForNegativeInput(int[] array) {
        for (int number : array) {
            if (number < 0) {
                throw new IllegalArgumentException("Array contains non-positive integers.");
            }
        }
    }

    private static void radixSort(int[] array) {
        final int max = Arrays.stream(array).max().getAsInt();
        for (int i = 0, exp = 1; i < NumberOfDigits.numberOfDigits(max); i++, exp *= BASE) {
            countingSortByDigit(array, exp);
        }
    }

    private static void countingSortByDigit(int[] array, int exp) {
        int[] count = countDigits(array, exp);
        accumulateCounts(count);
        int[] output = buildOutput(array, exp, count);
        copyOutput(array, output);
    }

    private static int[] countDigits(int[] array, int exp) {
        int[] count = new int[BASE];
        for (int i = 0; i < array.length; i++) {
            count[getDigit(array[i], exp)]++;
        }
        return count;
    }

    private static int getDigit(int number, int position) {
        return (number / position) % BASE;
    }

    private static void accumulateCounts(int[] count) {
        for (int i = 1; i < BASE; i++) {
            count[i] += count[i - 1];
        }
    }

    private static int[] buildOutput(int[] array, int exp, int[] count) {
        int[] output = new int[array.length];
        for (int i = array.length - 1; i >= 0; i--) {
            int digit = getDigit(array[i], exp);
            output[count[digit] - 1] = array[i];
            count[digit]--;
        }
        return output;
    }

    private static void copyOutput(int[] array, int[] output) {
        System.arraycopy(output, 0, array, 0, array.length);
    }
}
```

### Implementation Features

| Feature | Value |
|---------|-------|
| **BASE** | 10 (decimal) |
| **Input validation** | Rejects negative numbers |
| **Stable subroutine** | CountingSort per digit |
| **Modular design** | Well-separated helper methods |

---

## 🎯 Step-by-Step Example

### Input: `[170, 45, 75, 90, 802, 24, 2, 66]`

**Find max:** 802 → 3 digits → 3 passes

---

**Pass 1: Sort by Units Digit (exp=1)**

```
Extract units: [0, 5, 5, 0, 2, 4, 2, 6]
               170 45 75 90 802 24 2 66

Counting Sort by units digit:
  digit 0: [170, 90]
  digit 2: [802, 2]
  digit 4: [24]
  digit 5: [45, 75]
  digit 6: [66]

After Pass 1: [170, 90, 802, 2, 24, 45, 75, 66]
```

---

**Pass 2: Sort by Tens Digit (exp=10)**

```
Extract tens: [7, 9, 0, 0, 2, 4, 7, 6]
              170 90 802 2 24 45 75 66

Counting Sort by tens digit:
  digit 0: [802, 2]
  digit 2: [24]
  digit 4: [45]
  digit 6: [66]
  digit 7: [170, 75]
  digit 9: [90]

After Pass 2: [802, 2, 24, 45, 66, 170, 75, 90]
```

---

**Pass 3: Sort by Hundreds Digit (exp=100)**

```
Extract hundreds: [8, 0, 0, 0, 0, 1, 0, 0]
                  802 2 24 45 66 170 75 90

Counting Sort by hundreds digit:
  digit 0: [2, 24, 45, 66, 75, 90]
  digit 1: [170]
  digit 8: [802]

After Pass 3: [2, 24, 45, 66, 75, 90, 170, 802]
```

### Final Result: `[2, 24, 45, 66, 75, 90, 170, 802]` ✅

---

## 📊 Complexity Analysis

### Time Complexity

| Component | Complexity |
|-----------|------------|
| Find max | O(n) |
| Number of passes | d = O(log₁₀(max)) |
| Each pass (CountingSort) | O(n + k) |
| **Total** | **O(d × (n + k))** |

For base 10 with 32-bit integers: $d ≤ 10$, so $T(n) = O(n)$

### Space Complexity

| Component | Size |
|-----------|------|
| Count array | O(k) = O(10) |
| Output array | O(n) |
| **Total** | **O(n + k)** |

### RadixSort vs Comparison Sorts

| Algorithm | Time | Better When |
|-----------|------|-------------|
| **RadixSort** | O(d(n+k)) | d is small, k is small |
| QuickSort | O(n log n) | General purpose |
| MergeSort | O(n log n) | Need stability |

**RadixSort wins when:**
$$
d \cdot (n + k) < n \log n
$$

For example, with $k=10$ and $d=10$ (32-bit integers):
- RadixSort: ~10n operations
- QuickSort: ~n log n ≈ 20n (for n=1M)

---

## ⚠️ Common Pitfalls

### 1. Forgetting Stability

```java
// Wrong: forward traversal destroys stability
for (int i = 0; i < array.length; i++) {
    output[count[digit]++] = array[i];
}

// Correct: backward traversal maintains stability
for (int i = array.length - 1; i >= 0; i--) {
    output[--count[digit]] = array[i];
}
```

### 2. Incorrect Digit Extraction

```java
// Wrong: doesn't handle position correctly
int digit = number % BASE;  // Always gets units digit

// Correct: divide first, then modulo
int digit = (number / exp) % BASE;
```

### 3. Handling Negative Numbers

This implementation throws an exception for negatives. To support negatives:
```java
// Option 1: Separate positive/negative, sort, combine
// Option 2: Offset all values by minimum
// Option 3: Use MSD RadixSort for signed integers
```

---

## 🔧 Optimizations

### 1. Use Larger Base (Byte-wise)

```java
// Base 256 (byte-wise) for 32-bit integers
private static final int BASE = 256;  // Only 4 passes needed!

private static int getDigit(int number, int byteIndex) {
    return (number >> (byteIndex * 8)) & 0xFF;
}
```

### 2. Avoid Unnecessary Passes

```java
// Skip passes if max number doesn't have that digit
if (max < exp) break;  // All remaining digits are 0
```

### 3. In-Place MSD RadixSort

For strings or when memory is constrained:
```java
// MSD variant sorts in-place using recursion
void msdRadixSort(String[] arr, int lo, int hi, int d) {
    // Process digit d, recursively sort each bucket
}
```

---

## 🌍 Real-World Applications

| Application | Why RadixSort? |
|-------------|----------------|
| **Suffix arrays** | Fast string processing |
| **IP address sorting** | 4 bytes, fixed size |
| **Database indexing** | Integer key sorting |
| **Graphics** | Depth sorting (Z-buffer) |
| **Network packets** | Priority queue by numeric ID |

### Industry Usage

| Domain | Example |
|--------|---------|
| **Databases** | Oracle, PostgreSQL for integer columns |
| **Big Data** | Spark/Hadoop for numeric partitioning |
| **Gaming** | Rendering pipeline sorting |
| **Networking** | Packet classification |

---

## 🧪 Testing

### Test File

**Location:** [src/test/java/com/thealgorithms/sorts/RadixSortTest.java](../../src/test/java/com/thealgorithms/sorts/RadixSortTest.java)

### Test Cases

| Input | Expected | Tests |
|-------|----------|-------|
| `[]` | `[]` | Empty array |
| `[5]` | `[5]` | Single element |
| `[170,45,75,90]` | `[45,75,90,170]` | Multiple digits |
| `[1,1,1]` | `[1,1,1]` | All same |
| `[-5,2,1]` | Exception | Negative input |
| `[0,0,0]` | `[0,0,0]` | All zeros |

---

## 📖 References

### Textbooks

- Cormen, T.H., et al. *"Introduction to Algorithms"* (CLRS), Chapter 8.3
- Sedgewick, R. *"Algorithms"*, Section 5.1

### Papers

- McIlroy, P.M., Bostic, K., McIlroy, M.D. *"Engineering Radix Sort"*, Computing Systems, 1993

---

## 🔗 Related Algorithms

| Algorithm | Relationship |
|-----------|-------------|
| [CountingSort](./countingsort.md) | Used as stable subroutine |
| [BucketSort](./bucketsort.md) | Similar distribution concept |
| [MSDRadixSort](./msd-radixsort.md) | Processes MSD first |

---

## 📊 Visualization

```
Input: [170, 45, 75, 90, 802, 24, 2, 66]

Pass 1 - Units digit (×1):
┌─────────────────────────────────────┐
│ 170→0  45→5  75→5  90→0             │
│ 802→2  24→4   2→2  66→6             │
├─────────────────────────────────────┤
│ Bucket 0: [170, 90]                 │
│ Bucket 2: [802, 2]                  │
│ Bucket 4: [24]                      │
│ Bucket 5: [45, 75]                  │
│ Bucket 6: [66]                      │
└─────────────────────────────────────┘
Result: [170, 90, 802, 2, 24, 45, 75, 66]

Pass 2 - Tens digit (×10):
┌─────────────────────────────────────┐
│ 170→7  90→9  802→0   2→0            │
│  24→2  45→4   75→7  66→6            │
├─────────────────────────────────────┤
│ Bucket 0: [802, 2]                  │
│ Bucket 2: [24]                      │
│ Bucket 4: [45]                      │
│ Bucket 6: [66]                      │
│ Bucket 7: [170, 75]                 │
│ Bucket 9: [90]                      │
└─────────────────────────────────────┘
Result: [802, 2, 24, 45, 66, 170, 75, 90]

Pass 3 - Hundreds digit (×100):
┌─────────────────────────────────────┐
│ 802→8   2→0  24→0  45→0             │
│  66→0 170→1  75→0  90→0             │
├─────────────────────────────────────┤
│ Bucket 0: [2, 24, 45, 66, 75, 90]   │
│ Bucket 1: [170]                     │
│ Bucket 8: [802]                     │
└─────────────────────────────────────┘
Result: [2, 24, 45, 66, 75, 90, 170, 802] ✅
```

---

[← Back to Sorting Algorithms](../README.md) | [Next: BucketSort →](./bucketsort.md)
