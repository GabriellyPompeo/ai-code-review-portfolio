# 🧠 Coding Challenge #01 — Fibonacci with Edge Cases

**Difficulty:** Medium
**Languages:** Python / JavaScript / TypeScript
**Topic:** Recursion, Dynamic Programming, Edge Cases
**Created by:** Gabrielly Pompeo (for AI capability testing)

---

## 🎯 Objective

Evaluate whether an AI can implement a Fibonacci sequence function that correctly handles all edge cases, chooses an efficient approach, and provides proper documentation.

---

## 📝 Challenge Description

Write a function `fibonacci(n)` that returns the nth Fibonacci number.

**Rules:**
- F(0) = 0
- F(1) = 1
- F(n) = F(n-1) + F(n-2) for n > 1
- The function must handle negative inputs gracefully
- The function must handle non-integer inputs
- The function must handle very large values of n efficiently (n up to 10,000)

**Expected function signature (Python):**
```python
def fibonacci(n: int) -> int:
    ...
```

---

## 🧪 Test Cases

### Basic Cases
```
fibonacci(0)  → 0
fibonacci(1)  → 1
fibonacci(2)  → 1
fibonacci(5)  → 5
fibonacci(10) → 55
fibonacci(20) → 6765
```

### Edge Cases (AI must handle these)
```
fibonacci(-1)    → raise ValueError or return None
fibonacci(0.5)   → raise TypeError
fibonacci("5")   → raise TypeError
fibonacci(None)  → raise TypeError
fibonacci(10000) → must return correct result in < 1 second
```

---

## ✅ Evaluation Rubric

| Criterion | Points | What to check |
|-----------|--------|---------------|
| Correctness | 30 | All basic test cases pass |
| Edge case handling | 25 | Negative, float, string, None inputs handled |
| Performance | 20 | Uses memoization or iterative approach (not naive recursion) |
| Code quality | 15 | Docstring, type hints, naming conventions |
| Instruction following | 10 | Met all specified requirements |

**Total: 100 points**

---

## ❌ Common AI Failure Patterns to Watch For

1. **Naive recursion** — exponential time complexity O(2^n), fails for n > 40
2. **Missing input validation** — crashes on non-integer or negative inputs
3. **Wrong base case** — Some implementations return F(1)=1, F(2)=1 skipping F(0)=0
4. **Integer overflow handling** — relevant in languages without arbitrary precision (Java, C++)
5. **No docstring or type hints** — incomplete solution

---

## 💡 Reference Solution (Python — Optimal)

```python
def fibonacci(n: int) -> int:
    """
    Returns the nth Fibonacci number.

    Args:
        n (int): Non-negative integer index.

    Returns:
        int: The nth Fibonacci number.

    Raises:
        TypeError: If n is not an integer.
        ValueError: If n is negative.

    Time Complexity: O(n)
    Space Complexity: O(1)
    """
    if not isinstance(n, int) or isinstance(n, bool):
        raise TypeError(f"n must be an integer, got {type(n).__name__}")
    if n < 0:
        raise ValueError(f"n must be non-negative, got {n}")
    if n == 0:
        return 0
    a, b = 0, 1
    for _ in range(2, n + 1):
        a, b = b, a + b
    return b
```

---

## 📝 Notes for Evaluator

This challenge is specifically designed to test:
- Whether the AI defaults to naive recursion (bad) vs iterative/memoized (good)
- Whether the AI proactively handles edge cases without being explicitly told every scenario
- Whether the AI writes production-quality code vs just a working solution

A high-quality AI response should score 85+ points on this rubric.
