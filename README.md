# Blog_microservice 🚀

**FastAPI-based Blog microservice** with JWT authentication, SQLite , and simple user/blog CRUD endpoints.

---

## 🔧 Tech stack

- Python 3.8+
- FastAPI
- Uvicorn
- SQLAlchemy (SQLite)
- passlib / bcrypt
- python-jose (JWT)

---

## ✅ Features

- Create and fetch users
- JWT-based login to obtain access tokens
- Authenticated CRUD for blog posts
- SQLite database (automatic creation/migrations via SQLAlchemy models)

---

## 🧱 Project Structure

```
Blog_microservice/
├─ app/
│  ├─ main.py                # FastAPI app entry
│  ├─ requirements.txt
│  └─ blog/
│     ├─ database.py         # DB setup (sqlite:///./blog.db)
│     ├─ models.py           # SQLAlchemy models
│     ├─ schemas.py          # Pydantic schemas
│     ├─ token.py            # JWT token helpers
│     ├─ oauth2.py           # OAuth2 helper
│     ├─ hashing.py          # password hashing
│     ├─ repository/         # business logic (user/blog)
│     └─ routers/            # API routers (user, blog, auth)
```

---

## 🛠 Prerequisites

- Python 3.8+
- pip

---

## ⚡ Quickstart (development)

1. Create & activate a virtual environment

```bash
python -m venv .venv
# Windows
.venv\Scripts\activate
# macOS / Linux
source .venv/bin/activate
```

2. Install dependencies

```bash
cd app
pip install -r requirements.txt
```

3. Start the server (from `app/`)

```bash
uvicorn main:app --reload --host 0.0.0.0 --port 8000
```

4. API docs

- Open interactive docs: http://127.0.0.1:8000/docs
- Redoc: http://127.0.0.1:8000/redoc

---

## 📡 API Endpoints

Authentication

- POST /login (form): obtain access token
  - form fields: `username` (email), `password`
  - returns: `{ "access_token": "<token>", "token_type": "bearer" }`

Users

- POST /user
  - JSON body: `{ "name": "Name", "email": "email@example.com", "password": "pass" }`
  - returns created user (without password)

- GET /user/{id}
  - public (no auth)

Blogs (require Bearer token)

- GET /blog/
  - list all blogs (authenticated)

- POST /blog/
  - JSON body: `{ "title": "My Post", "body": "Content" }`
  - requires Bearer token

- GET /blog/{id}
- PUT /blog/{id}
- DELETE /blog/{id}

Use Authorization header:

```
Authorization: Bearer <access_token>
```

Example: login and use token with cURL

```bash
# Login (form data) -> receive access token
curl -X POST -F "username=user@example.com" -F "password=secret" http://127.0.0.1:8000/login

# Use token to fetch blogs
curl -H "Authorization: Bearer <token>" http://127.0.0.1:8000/blog/
```

---

## 🧾 Database

- Default DB: SQLite at `./blog.db` (relative to where the server is started)
- Tables are created automatically on startup (see `main.py`)

---

## ⚠️ Known issues & notes

- `blog.repository.create` currently hardcodes `user_id=1`. In a multi-user setup, update it to use the authenticated user's id (passed via dependency `current_user`).
- `blog/token.verify_token` currently raises on invalid token but does not return the decoded token data on success — you may want to return the `TokenData` or user identifier for `get_current_user` usage.
- SECRET_KEY and other sensitive values are stored in code. For production, move these into environment variables and a config management solution.

---

## 🧪 Testing

- No test suite is included. Add pytest and tests under a `tests/` directory for unit/integration tests.

---


