# 🐛 Bug Report #01 — Logic Error in Python AI-Generated Code

**Report ID:** BUG-001
**Date:** 2026-05-08
**Reporter:** Gabrielly Pompeo
**Severity:** High
**Status:** Confirmed
**Language:** Python 3

---

## 📌 Summary

AI-generated function for finding the maximum value in a nested list returns incorrect results for lists containing negative numbers only. The bug causes the function to return `0` (the initialized value) instead of the actual maximum.

---

## 🤖 AI-Generated Code (Buggy)

```python
def find_max_nested(nested_list):
    """Find the maximum value in a nested list."""
    max_val = 0  # BUG: initialized to 0, not to negative infinity
    for sublist in nested_list:
        for item in sublist:
            if item > max_val:
                max_val = item
    return max_val
```

---

## 🧪 Steps to Reproduce

```python
# Test case that reveals the bug
nested = [[-5, -3], [-10, -1]]
result = find_max_nested(nested)
print(result)  # Output: 0
               # Expected: -1
```

---

## ❌ Actual vs Expected Behavior

| Input | Expected Output | Actual Output | Status |
|-------|----------------|---------------|--------|
| `[[1, 2], [3, 4]]` | `4` | `4` | ✅ Pass |
| `[[-5, -3], [-10, -1]]` | `-1` | `0` | ❌ FAIL |
| `[[0, 0], [0, 0]]` | `0` | `0` | ✅ Pass (accidentally) |
| `[[-100], [-200]]` | `-100` | `0` | ❌ FAIL |
| `[]` | `None` or raise | `0` | ❌ FAIL |

---

## 🔍 Root Cause Analysis

**Root Cause:** The variable `max_val` is initialized to `0` instead of negative infinity (`float('-inf')`) or the first element of the list.

This is a classic programming error. When all values in the list are negative, none of them will be greater than `0`, so `max_val` is never updated and the function incorrectly returns `0`.

**Instruction Following Failure:** The AI did not follow the implicit requirement that the function should work for all valid numeric inputs, including negative-only lists.

---

## 💡 Fix

```python
def find_max_nested(nested_list: list) -> float | None:
    """
    Find the maximum value in a nested list.

    Args:
        nested_list (list): A list of lists containing numbers.

    Returns:
        float | None: The maximum value, or None if the list is empty.

    Raises:
        TypeError: If input is not a list.
    """
    if not isinstance(nested_list, list):
        raise TypeError("Input must be a list")

    max_val = float('-inf')  # FIX: initialize to negative infinity
    found = False

    for sublist in nested_list:
        for item in sublist:
            if item > max_val:
                max_val = item
                found = True

    return max_val if found else None  # FIX: handle empty list
```

---

## 📈 Severity Assessment

- **Severity:** High
- **Impact:** Silent incorrect output — no error is raised, making the bug hard to detect in production
- **Affected scenarios:** Any input containing only negative numbers
- **Detection difficulty:** Medium — only appears with specific negative-only inputs

---

## 📝 Structured Feedback to AI

The implementation has a critical logic error: `max_val` is initialized to `0` instead of `float('-inf')`. This causes the function to silently return an incorrect result (`0`) whenever all values in the nested list are negative. The fix is to initialize `max_val = float('-inf')` and separately track whether any element was found to properly handle empty inputs. Additionally, the function lacks input type validation and empty-list handling. This type of initialization bug is one of the most common errors in AI-generated code for min/max problems and should always be checked during review.
