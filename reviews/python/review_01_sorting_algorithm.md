# 🔍 Code Review #01 — Sorting Algorithm (Python)

**Date:** 2026-05-08
**Language:** Python 3
**Difficulty:** Medium
**Source:** AI-generated code (simulated ChatGPT output)
**Overall Rating:** 3/5 ⭐⭐⭐

---

## 📝 Original Prompt Given to AI

> "Write a Python function that takes a list of integers and returns it sorted in ascending order without using the built-in `sort()` or `sorted()` functions."

---

## 🤖 AI-Generated Code

```python
def sort_list(numbers):
    for i in range(len(numbers)):
        for j in range(len(numbers) - 1):
            if numbers[j] > numbers[j + 1]:
                numbers[j], numbers[j + 1] = numbers[j + 1], numbers[j]
    return numbers
```

---

## ✅ What the AI Did Well

- Correctly implemented Bubble Sort logic
- Properly uses tuple unpacking for swapping (Pythonic)
- Returns the modified list
- Follows the constraint of not using `sort()` or `sorted()`

---

## ❌ Issues Found

### Bug #1 — Mutates the Input List
**Severity:** Medium
**Line:** 1 (function signature)

The function modifies the original list passed as argument (in-place mutation). This is unexpected behavior for a function that appears to return a "sorted" copy.

```python
# Problematic behavior:
original = [3, 1, 2]
result = sort_list(original)
print(original)  # [1, 2, 3] ← original was also changed!
```

### Bug #2 — No Input Validation
**Severity:** Low
**Line:** 1 (function signature)

The function doesn't validate the input. Passing `None`, a string, or a list with non-comparable types will raise an unhandled `TypeError`.

```python
sort_list(None)       # TypeError: object of type 'NoneType' has no len()
sort_list([1, "a"])  # TypeError: '>' not supported between instances
```

### Issue #3 — Inefficient Algorithm (No Early Exit)
**Severity:** Low-Medium
**Line:** 2-3 (outer loop)

The outer loop always runs `n` times even if the list becomes sorted early. A simple flag can reduce unnecessary iterations.

### Issue #4 — No Docstring
**Severity:** Low

The function lacks documentation explaining parameters, return value, and complexity.

---

## 💡 Suggested Improved Version

```python
def sort_list(numbers: list) -> list:
    """
    Sorts a list of integers in ascending order using Bubble Sort.
    Does NOT modify the original list.

    Args:
        numbers (list): A list of comparable elements.

    Returns:
        list: A new sorted list.

    Raises:
        TypeError: If input is not a list.

    Time Complexity: O(n^2) average/worst, O(n) best case
    Space Complexity: O(n)
    """
    if not isinstance(numbers, list):
        raise TypeError(f"Expected a list, got {type(numbers).__name__}")

    arr = numbers.copy()  # Avoid mutating original
    n = len(arr)

    for i in range(n):
        swapped = False  # Early exit optimization
        for j in range(n - i - 1):  # Fixed: reduces inner loop range
            if arr[j] > arr[j + 1]:
                arr[j], arr[j + 1] = arr[j + 1], arr[j]
                swapped = True
        if not swapped:
            break  # List already sorted

    return arr
```

---

## 📊 Evaluation Summary

| Criterion | Score | Notes |
|-----------|-------|-------|
| Correctness | 4/5 | Works for happy path, fails edge cases |
| Efficiency | 2/5 | Missing early exit optimization |
| Best Practices | 3/5 | No docstring, mutates input |
| Edge Cases | 1/5 | No input validation |
| Readability | 4/5 | Clean and easy to follow |
| Instruction Following | 5/5 | Met the core requirement |

**Overall: 3.2/5**

---

## 📝 Structured Feedback to AI

The solution correctly implements Bubble Sort and meets the core requirement of avoiding built-in sort functions. However, it has three notable issues: (1) it mutates the input list which is a common Python anti-pattern for functions that appear to return a value; (2) it lacks input validation for non-list types; and (3) the outer loop is missing an early-exit optimization that would improve best-case performance from O(n²) to O(n). Additionally, the function has no docstring. The fix is straightforward: copy the input list before sorting, add a `swapped` flag, adjust the inner loop range to `n - i - 1` (avoiding re-checking already-sorted elements), and add type validation and documentation.
