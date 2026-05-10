# 🤖 AI Code Review Portfolio

> **Gabrielly Pompeo da Costa** — QA Engineer | AI Code Reviewer | React Native Developer

[![LinkedIn](https://img.shields.io/badge/LinkedIn-gabrielly--pompeo-blue?style=flat&logo=linkedin)](https://linkedin.com/in/gabrielly-pompeo-7ba312160)
[![GitHub](https://img.shields.io/badge/GitHub-GabriellyPompeo-black?style=flat&logo=github)](https://github.com/GabriellyPompeo)

---

## 📌 About This Repository

This portfolio demonstrates my ability to **review, evaluate, and improve AI-generated code** — a core skill for AI training and software quality roles. It contains:

- ✅ **Code Reviews** — detailed analysis of AI-generated code in Python, JavaScript, TypeScript, and Java
- 💭 **Coding Challenges** — original computer science questions designed to test AI capabilities
- 🐞 **Bug Reports** — structured documentation of bugs and security vulnerabilities found in AI outputs

---

## 📂 Repository Structure

```
ai-code-review-portfolio/
├── reviews/
│   ├── python/
│   │   └── review_01_sorting_algorithm.md
│   ├── javascript/
│   │   └── review_01_async_await.md
│   ├── typescript/
│   │   └── review_01_type_safety_generics.md
│   └── java/
│       └── review_01_singleton_pattern.md
├── challenges/
│   ├── challenge_01_fibonacci_edge_cases.md
│   ├── challenge_02_rest_api_design.md
│   └── challenge_03_algorithm_complexity.md
└── bug-reports/
    ├── bug_01_logic_error_python.md
    └── bug_02_xss_vulnerability_javascript.md
```

---

## 📊 Portfolio Summary

| Category | Count | Languages / Topics |
|---|---|---|
| Code Reviews | 4 | Python, JavaScript, TypeScript, Java |
| Coding Challenges | 3 | Edge Cases, REST API Design, Algorithm Complexity |
| Bug Reports | 2 | Logic Error (Python), XSS Vulnerability (JavaScript) |
| **Total files** | **9** | **4 languages covered** |

---

## 🔍 Review Methodology

Every code review in this portfolio evaluates AI-generated output across 6 dimensions:

| Dimension | Description |
|---|---|
| **Correctness** | Does the code produce the expected output for all inputs? |
| **Efficiency** | Is the time and space complexity appropriate? |
| **Instruction Following** | Did the AI address all constraints in the prompt? |
| **Edge Cases** | Are boundary conditions, null inputs, and failure modes handled? |
| **Best Practices** | Does the code follow conventions for the language/framework? |
| **Security** | Are there vulnerabilities such as injection, hardcoded credentials, or data exposure? |

---

## 🐛 Common AI Failure Patterns I Identify

Through hands-on evaluation of AI-generated code, I have documented recurring failure patterns:

- **Logic errors** — wrong initialization values, off-by-one errors, incorrect operators
- **Performance anti-patterns** — sequential `await` instead of `Promise.all()`, O(n²) where O(n) exists
- **Missing edge cases** — null inputs, empty arrays, negative numbers not handled
- **Silent failures** — exceptions swallowed in `catch` blocks, returning `undefined`
- **Instruction-following gaps** — AI ignoring explicit constraints in the prompt (e.g., thread-safety)
- **Security vulnerabilities** — XSS via `innerHTML`, hardcoded credentials, missing input validation
- **Missing documentation** — no docstrings, type hints, or input validation

---

## 🤖 AI Tools Experience

I actively use AI tools both in personal projects and in professional QA workflows:

| Tool | How I Use It |
|---|---|
| **Claude (Anthropic)** | Code review, structured feedback, technical documentation, Jira task comments |
| **ChatGPT (OpenAI)** | Code generation review, prompt testing, edge case exploration |
| **GitHub Copilot** | Inline code suggestion evaluation in real development workflows |
| **Gemini (Google)** | Code explanation, debugging assistance |
| **Perplexity AI** | Research, technical documentation, competitive analysis |

> I use these tools **critically** — identifying when they produce correct, incorrect, or incomplete outputs — which directly aligns with AI training and evaluation workflows. I also use AI to craft and refine prompts, enabling more precise and useful outputs.

---

## 🚀 Featured Reviews

| File | Language | Key Bugs Found | Score |
|---|---|---|---|
| [Singleton Pattern](reviews/java/review_01_singleton_pattern.md) | Java | Race condition, hardcoded credentials, silent exception | 3.2/10 |
| [Async/Await Handler](reviews/javascript/review_01_async_await.md) | JavaScript | Sequential await, missing error handling, no input validation | 3.5/10 |
| [Sorting Algorithm](reviews/python/review_01_sorting_algorithm.md) | Python | Max value init bug, no edge case handling | 4.0/10 |
| [Type Safety & Generics](reviews/typescript/review_01_type_safety_generics.md) | TypeScript | Missing generics, `any[]` usage, no type guards | 3.7/10 |

---

## 🛡️ Featured Bug Reports

| File | Severity | Vulnerability Type |
|---|---|---|
| [XSS via innerHTML](bug-reports/bug_02_xss_vulnerability_javascript.md) | **Critical** (CVSS 8.2) | Cross-Site Scripting in AI-generated DOM manipulation |
| [Logic Error - Max Value](bug-reports/bug_01_logic_error_python.md) | High | Wrong initialization causing incorrect results |

---

[![GitHub Stats](https://github-readme-stats.vercel.app/api?username=GabriellyPompeo&show_icons=true&theme=dark&hide_border=true)](https://github.com/GabriellyPompeo)
