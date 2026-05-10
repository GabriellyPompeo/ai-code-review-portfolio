# Coding Challenge #03 — Algorithm Complexity & Optimization

**Difficulty:** Intermediate  
**Language:** Python  
**Topic:** Time Complexity, Space Complexity, Big O Notation  
**Date:** 2026-05-10  
**Author:** Gabrielly Pompeo da Costa

---

## Challenge Overview

This challenge tests whether an AI model can:
1. Identify inefficient algorithms and their time/space complexity
2. Optimize code to achieve better asymptotic performance
3. Recognize common anti-patterns that cause O(n²) or worse behavior
4. Apply appropriate data structures to reduce complexity

---

## Problem Statement

You are given the following Python functions. For **each function**:
- State the **time complexity** (Big O notation)
- State the **space complexity**
- Identify **what the bottleneck is** (if any)
- Write an **optimized version** with better complexity where possible
- Add **unit tests** covering edge cases

---

## Function 1: Find Duplicates

```python
def find_duplicates(items: list) -> list:
    """Return all duplicate values in the list."""
    duplicates = []
    for i in range(len(items)):
        for j in range(i + 1, len(items)):
            if items[i] == items[j] and items[i] not in duplicates:
                duplicates.append(items[i])
    return duplicates
```

**Questions:**
1. What is the time complexity of this function?
2. What data structure would reduce this to O(n)?
3. Write the optimized version.

---

## Function 2: Two Sum

```python
def two_sum(numbers: list, target: int) -> tuple:
    """Return indices of two numbers that add up to target."""
    for i in range(len(numbers)):
        for j in range(len(numbers)):
            if i != j and numbers[i] + numbers[j] == target:
                return (i, j)
    return None
```

**Questions:**
1. What is the time complexity?
2. There is a redundant comparison being made — identify it.
3. Write the O(n) solution using a hash map.

---

## Function 3: Check Anagram

```python
def is_anagram(word1: str, word2: str) -> bool:
    """Return True if word1 and word2 are anagrams."""
    if len(word1) != len(word2):
        return False
    for char in word1:
        if word1.count(char) != word2.count(char):
            return False
    return True
```

**Questions:**
1. What is the time complexity? (Hint: `str.count()` is O(n))
2. What is the optimized approach using a Counter or sorted comparison?
3. Which edge cases does this function fail to handle?

---

## Function 4: Flatten Nested List

```python
def flatten(nested: list) -> list:
    """Flatten a nested list of arbitrary depth."""
    result = []
    for item in nested:
        if isinstance(item, list):
            result = result + flatten(item)  # note: using +
        else:
            result.append(item)
    return result
```

**Questions:**
1. Why is `result = result + flatten(item)` worse than `result.extend(flatten(item))`?
2. What is the hidden performance issue with list concatenation in Python?
3. Rewrite using a generator for memory efficiency.

---

## Evaluation Rubric (100 points)

| Criterion | Points | Description |
|---|---|---|
| Complexity Analysis | 30 | Correct Big O for each function, time AND space |
| Optimization Correctness | 30 | Optimized versions produce same output |
| Edge Case Coverage | 20 | Empty input, single element, duplicates, negative numbers |
| Code Quality | 10 | Type hints, docstrings, naming conventions |
| Test Coverage | 10 | At least 3 test cases per function |

---

## Reference Solutions

### Function 1 — Optimized

```python
# Original: O(n³) — nested loop O(n²) + `in` check on list O(n)
# Optimized: O(n) using sets

def find_duplicates_optimized(items: list) -> list:
    """
    Return all duplicate values in the list.
    Time: O(n) | Space: O(n)
    """
    seen = set()
    duplicates = set()
    for item in items:
        if item in seen:
            duplicates.add(item)
        else:
            seen.add(item)
    return list(duplicates)
```

### Function 2 — Optimized

```python
# Original: O(n²) — also checks (j, i) after (i, j) needlessly
# Optimized: O(n) using hash map

def two_sum_optimized(numbers: list, target: int) -> tuple | None:
    """
    Return indices of two numbers that sum to target.
    Time: O(n) | Space: O(n)
    """
    seen = {}  # value -> index
    for i, num in enumerate(numbers):
        complement = target - num
        if complement in seen:
            return (seen[complement], i)
        seen[num] = i
    return None
```

### Function 3 — Optimized

```python
# Original: O(n²) — for each char, .count() scans entire string again
# Optimized: O(n log n) using sorted, or O(n) using Counter

from collections import Counter

def is_anagram_optimized(word1: str, word2: str) -> bool:
    """
    Return True if word1 and word2 are anagrams.
    Time: O(n) | Space: O(k) where k = unique characters
    
    Edge cases handled: case sensitivity, spaces, unicode
    """
    if not isinstance(word1, str) or not isinstance(word2, str):
        raise TypeError("Both arguments must be strings")
    return Counter(word1.lower()) == Counter(word2.lower())
```

### Function 4 — Optimized (Generator)

```python
# Original: O(n²) in worst case due to list concatenation creating new lists
# Optimized: O(n) generator approach

from typing import Generator, Any

def flatten_optimized(nested: list) -> Generator[Any, None, None]:
    """
    Flatten a nested list of arbitrary depth using a generator.
    Time: O(n) | Space: O(d) where d = nesting depth
    """
    for item in nested:
        if isinstance(item, list):
            yield from flatten_optimized(item)
        else:
            yield item

# Usage: list(flatten_optimized([1, [2, [3, 4]], 5]))
```

---

## Common AI Failure Patterns for This Challenge

| Failure | Description |
|---|---|
| Ignoring `str.count()` cost | AI often treats `str.count()` as O(1) when it’s O(n) |
| Missing space complexity | AI frequently analyzes only time, not space |
| List concatenation trap | AI uses `result = result + [item]` creating O(n²) behavior |
| Incorrect hash map indexing | AI sometimes returns values instead of indices in Two Sum |
| No edge case handling | Empty lists, `None` input, single-element lists ignored |

---

## Test Cases

```python
import pytest

# --- find_duplicates ---
def test_find_duplicates_empty():
    assert find_duplicates_optimized([]) == []

def test_find_duplicates_no_dupes():
    assert find_duplicates_optimized([1, 2, 3]) == []

def test_find_duplicates_multiple():
    result = find_duplicates_optimized([1, 2, 2, 3, 3, 3])
    assert set(result) == {2, 3}

# --- two_sum ---
def test_two_sum_found():
    assert two_sum_optimized([2, 7, 11, 15], 9) == (0, 1)

def test_two_sum_not_found():
    assert two_sum_optimized([1, 2, 3], 100) is None

def test_two_sum_negative_numbers():
    assert two_sum_optimized([-3, 4, 3, 90], 0) == (0, 2)

# --- is_anagram ---
def test_anagram_true():
    assert is_anagram_optimized("listen", "silent") is True

def test_anagram_false():
    assert is_anagram_optimized("hello", "world") is False

def test_anagram_case_insensitive():
    assert is_anagram_optimized("Listen", "Silent") is True

# --- flatten ---
def test_flatten_nested():
    assert list(flatten_optimized([1, [2, [3, 4]], 5])) == [1, 2, 3, 4, 5]

def test_flatten_empty():
    assert list(flatten_optimized([])) == []

def test_flatten_single_level():
    assert list(flatten_optimized([1, 2, 3])) == [1, 2, 3]
```
