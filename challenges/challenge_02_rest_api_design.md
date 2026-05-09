# 🎯 Coding Challenge #02 — REST API Design

**Author:** Gabrielly Pompeo da Costa — QA Engineer | AI Code Reviewer
**Difficulty:** Intermediate
**Skills Tested:** REST API design, HTTP methods, status codes, error handling, data validation
**Estimated Time:** 45–60 minutes

---

## 📝 Challenge Description

You are tasked with designing and implementing a simple **User Management REST API** in Python using any framework (Flask, FastAPI, or plain HTTP). The API must follow REST principles and handle edge cases properly.

### Requirements:

1. Create a REST API with the following endpoints:
   - `GET /users` — returns a list of all users
   - `GET /users/{id}` — returns a specific user by ID
   - `POST /users` — creates a new user
   - `PUT /users/{id}` — updates an existing user
   - `DELETE /users/{id}` — deletes a user

2. Each user should have:
   - `id` (auto-generated, integer)
   - `name` (string, required, min 2 chars, max 100 chars)
   - `email` (string, required, must be valid email format)
   - `age` (integer, optional, must be between 0 and 150)
   - `created_at` (ISO 8601 datetime, auto-generated)

3. The API must:
   - Return appropriate HTTP status codes
   - Validate all input data
   - Return consistent JSON error responses
   - Handle non-existent resources gracefully

---

## 🧩 Test Cases

### TC-01: List all users (empty database)
```
GET /users
Expected: 200 OK
Body: { "users": [], "total": 0 }
```

### TC-02: Create user with valid data
```
POST /users
Body: { "name": "Alice Silva", "email": "alice@example.com", "age": 28 }
Expected: 201 Created
Body: { "id": 1, "name": "Alice Silva", "email": "alice@example.com", "age": 28, "created_at": "..." }
```

### TC-03: Create user with missing required field
```
POST /users
Body: { "name": "Bob" }  // missing email
Expected: 422 Unprocessable Entity
Body: { "error": "Validation failed", "details": [{ "field": "email", "message": "Field is required" }] }
```

### TC-04: Create user with invalid email format
```
POST /users
Body: { "name": "Charlie", "email": "not-an-email" }
Expected: 422 Unprocessable Entity
Body: { "error": "Validation failed", "details": [{ "field": "email", "message": "Invalid email format" }] }
```

### TC-05: Create user with invalid age
```
POST /users
Body: { "name": "Diana", "email": "diana@example.com", "age": -5 }
Expected: 422 Unprocessable Entity
Body: { "error": "Validation failed", "details": [{ "field": "age", "message": "Age must be between 0 and 150" }] }
```

### TC-06: Get user by ID (exists)
```
GET /users/1
Expected: 200 OK
Body: { "id": 1, "name": "Alice Silva", ... }
```

### TC-07: Get user by ID (not found)
```
GET /users/999
Expected: 404 Not Found
Body: { "error": "User not found", "id": 999 }
```

### TC-08: Get user with invalid ID format
```
GET /users/abc
Expected: 400 Bad Request
Body: { "error": "Invalid ID format", "message": "ID must be a positive integer" }
```

### TC-09: Update user (full update)
```
PUT /users/1
Body: { "name": "Alice Santos", "email": "alice.santos@example.com", "age": 29 }
Expected: 200 OK
Body: { "id": 1, "name": "Alice Santos", "email": "alice.santos@example.com", "age": 29, "created_at": "..." }
```

### TC-10: Delete user (exists)
```
DELETE /users/1
Expected: 204 No Content
```

### TC-11: Delete user (already deleted)
```
DELETE /users/1
Expected: 404 Not Found
Body: { "error": "User not found", "id": 1 }
```

### TC-12: Duplicate email on create
```
POST /users
Body: { "name": "Eve", "email": "alice@example.com" }  // already taken
Expected: 409 Conflict
Body: { "error": "Email already in use", "email": "alice@example.com" }
```

---

## 📊 Evaluation Rubric (100 points)

| Criteria | Points | Description |
|----------|--------|-------------|
| Correct HTTP status codes | 20 | All 12 test cases use the right status code |
| Input validation | 20 | Name, email, age all validated with clear error messages |
| Consistent error format | 15 | All errors follow the same JSON structure |
| Edge case handling | 20 | Non-existent resources, invalid formats, duplicates |
| RESTful design principles | 15 | Proper use of HTTP methods, resource naming |
| Code organization | 10 | Clean, readable, separated validation logic |

---

## ⚠️ Common AI Failure Patterns in This Challenge

When asking AI assistants to complete this challenge, watch for:

1. **Wrong status codes** — AI often returns `200` for creation instead of `201 Created`
2. **Missing 409 Conflict** — duplicate email is frequently ignored
3. **Inconsistent error format** — some errors return strings, others return objects
4. **`400` instead of `422`** — for validation errors, `422 Unprocessable Entity` is more semantically correct
5. **Missing `total` field** — in list responses, AI often skips pagination metadata
6. **`id` not validated** — `GET /users/abc` often causes a 500 Internal Server Error instead of 400

---

## ✅ Reference Solution Snippet (Python/FastAPI)

```python
from fastapi import FastAPI, HTTPException
from pydantic import BaseModel, EmailStr, validator
from typing import Optional
from datetime import datetime

app = FastAPI()
users_db = {}
id_counter = 1

class UserCreate(BaseModel):
    name: str
    email: EmailStr
    age: Optional[int] = None

    @validator('name')
    def name_must_be_valid(cls, v):
        if len(v) < 2 or len(v) > 100:
            raise ValueError('Name must be between 2 and 100 characters')
        return v

    @validator('age')
    def age_must_be_valid(cls, v):
        if v is not None and (v < 0 or v > 150):
            raise ValueError('Age must be between 0 and 150')
        return v

@app.post('/users', status_code=201)  # ✔ 201 Created, not 200
def create_user(user: UserCreate):
    global id_counter
    # Check duplicate email
    for existing in users_db.values():
        if existing['email'] == user.email:
            raise HTTPException(status_code=409, detail={
                'error': 'Email already in use',
                'email': user.email
            })
    new_user = {
        'id': id_counter,
        **user.dict(),
        'created_at': datetime.utcnow().isoformat()
    }
    users_db[id_counter] = new_user
    id_counter += 1
    return new_user
```

---

> *This challenge was designed to test AI code evaluation skills — specifically the ability to identify REST API design errors, incorrect HTTP status codes, and missing edge case handling.*
