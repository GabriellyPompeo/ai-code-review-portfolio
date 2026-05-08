# 🔍 Code Review #01 — Async/Await & Promise Handling (JavaScript)

**Date:** 2026-05-08
**Language:** JavaScript (ES2017+)
**Difficulty:** Medium-Hard
**Source:** AI-generated code (simulated Copilot output)
**Overall Rating:** 2.5/5 ⭐⭐

---

## 📝 Original Prompt Given to AI

> "Write a JavaScript function that fetches user data from an API and their posts, then returns an object with both. Handle errors appropriately."

---

## 🤖 AI-Generated Code

```javascript
async function getUserWithPosts(userId) {
  try {
    const userResponse = await fetch(`https://jsonplaceholder.typicode.com/users/${userId}`);
    const user = await userResponse.json();

    const postsResponse = await fetch(`https://jsonplaceholder.typicode.com/posts?userId=${userId}`);
    const posts = await postsResponse.json();

    return { user, posts };
  } catch (error) {
    console.log('Error:', error);
  }
}
```

---

## ✅ What the AI Did Well

- Correctly uses `async/await` syntax instead of nested `.then()` chains
- Uses template literals for URL interpolation (clean)
- Wraps logic in `try/catch` block (shows awareness of error handling)
- Returns a structured object `{ user, posts }` using ES6 shorthand

---

## ❌ Issues Found

### Bug #1 — Sequential Requests Instead of Parallel (Performance)
**Severity:** High
**Lines:** 3–4 and 6–7

The two `fetch` calls run **sequentially** — the posts request only starts after the user request completes. Since they are independent, they should run **in parallel** using `Promise.all()`. This doubles the wait time unnecessarily.

```javascript
// Current behavior (sequential — BAD for performance):
// |--user fetch (300ms)--|--posts fetch (400ms)--| = 700ms total

// Expected behavior (parallel — GOOD):
// |--user fetch (300ms)--|
// |--posts fetch (400ms)--|  = 400ms total
```

### Bug #2 — HTTP Error Status Not Checked
**Severity:** High
**Lines:** 3–4

`fetch()` does **NOT** throw on HTTP 4xx/5xx errors. A 404 or 500 response is treated as a successful promise. The code must check `response.ok` before parsing JSON.

```javascript
// This will NOT be caught by try/catch:
const response = await fetch('.../users/99999'); // Returns 404
const data = await response.json();              // Parses {}, not an error!
```

### Bug #3 — Silent Error Swallowing
**Severity:** Medium
**Lines:** 10–12 (catch block)

The catch block only does `console.log('Error:', error)` and then **returns `undefined` implicitly**. The caller has no way to know if the function failed. This is a silent failure — one of the most dangerous patterns in JavaScript.

```javascript
// Caller cannot detect the error:
const result = await getUserWithPosts(99999);
console.log(result); // undefined — caller thinks it worked!
console.log(result.user); // TypeError: Cannot read properties of undefined
```

### Issue #4 — No Input Validation
**Severity:** Low
**Line:** 1

No validation of `userId`. Passing `undefined`, `null`, `0`, or a negative number creates an invalid URL silently.

### Issue #5 — No JSDoc / Type Annotations
**Severity:** Low

Missing documentation. The return type is unclear and parameter expectations are not documented.

---

## 💡 Suggested Improved Version

```javascript
/**
 * Fetches a user and their posts in parallel from the API.
 *
 * @param {number} userId - The ID of the user (must be a positive integer)
 * @returns {Promise<{user: Object, posts: Array}>} User data and posts
 * @throws {Error} If userId is invalid, or if any HTTP request fails
 */
async function getUserWithPosts(userId) {
  // Input validation
  if (!Number.isInteger(userId) || userId <= 0) {
    throw new TypeError(`userId must be a positive integer, got: ${userId}`);
  }

  // Helper to check HTTP response status
  async function fetchJSON(url) {
    const response = await fetch(url);
    if (!response.ok) {
      throw new Error(`HTTP ${response.status}: Failed to fetch ${url}`);
    }
    return response.json();
  }

  // Run both requests IN PARALLEL with Promise.all
  const [user, posts] = await Promise.all([
    fetchJSON(`https://jsonplaceholder.typicode.com/users/${userId}`),
    fetchJSON(`https://jsonplaceholder.typicode.com/posts?userId=${userId}`),
  ]);

  return { user, posts };
  // Note: errors are NOT silently swallowed - they propagate to the caller
}

// Usage example with proper error handling at the caller:
try {
  const { user, posts } = await getUserWithPosts(1);
  console.log(`User: ${user.name}, Posts: ${posts.length}`);
} catch (error) {
  console.error('Failed to load user data:', error.message);
}
```

---

## 📊 Evaluation Summary

| Criterion | Score | Notes |
|-----------|-------|-------|
| Correctness | 2/5 | Works for happy path only; fails on HTTP errors |
| Efficiency | 1/5 | Sequential fetches instead of parallel — serious performance flaw |
| Best Practices | 2/5 | Silent error swallowing is a major anti-pattern |
| Edge Cases | 1/5 | No `response.ok` check, no input validation |
| Readability | 4/5 | Clean syntax, easy to read |
| Instruction Following | 3/5 | Partially fulfilled — error handling is incomplete |

**Overall: 2.5/5**

---

## 📝 Structured Feedback to AI

The solution demonstrates correct `async/await` usage and basic `try/catch` awareness, but has three significant issues that would cause problems in production: (1) The two fetch calls run sequentially when they should run in parallel via `Promise.all()`, doubling the response time unnecessarily; (2) `fetch()` does not throw on HTTP error statuses — a 404 response must be explicitly checked via `response.ok` before calling `.json()`; (3) The catch block silently swallows the error by only logging it and returning `undefined`, making it impossible for the caller to detect failure. These are classic JavaScript async pitfalls. A revised version should use `Promise.all()` for parallel requests, validate the HTTP response status, and re-throw (or propagate) errors to the caller rather than absorbing them silently.
