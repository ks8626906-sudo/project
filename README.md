# Task Management REST API + Web UI

Node.js + Express + MySQL backend with JWT auth, and a plain HTML/CSS/JS front end served from `public/`.

## Setup
1. Install Node.js (18+) and MySQL Community Server (or XAMPP).
2. Create the database: `mysql -u root -p < schema.sql` (or run `schema.sql` in MySQL Workbench).
3. `npm install`
4. Copy `.env.example` to `.env` and fill in your MySQL password and a random `JWT_SECRET`.
5. `npm start`, then open http://localhost:3000

## API
Protected routes need the header `Authorization: Bearer <token>`.

| Method | Endpoint | Description | Auth |
|---|---|---|---|
| POST | /register | `{name,email,password}` create user (bcrypt hash) | No |
| POST | /login | `{email,password}` returns `{token,name}` | No |
| POST | /tasks | `{title,description,status,due_date}` create task | Yes |
| GET | /tasks | List own tasks. Query: `status`, `search` (title), `page`, `limit` | Yes |
| PUT | /tasks/:id | Update title, description, status, due_date | Yes |
| DELETE | /tasks/:id | Delete a task | Yes |

## Errors
| Case | Status | Message |
|---|---|---|
| Missing title | 400 | Title is required |
| Bad date | 400 | Due date must be YYYY-MM-DD |
| Duplicate email | 409 | Email already exists |
| Wrong password | 401 | Invalid credentials |
| No/invalid token | 401 | Please login first |
| Task not found or not yours | 404 | Task not found |

A user's tasks from another account return 404 (not 403) so task IDs are not leaked. All queries use `?` placeholders.

## Bonus features included
Search by title, pagination (5 per page), JWT auth, edit button in the UI.

## Running without MySQL (local testing only)
Set `DB_TYPE=sqlite` in `.env`, run `npm install`, then `npm start`. Data is stored in a local `tasks.db` file.
The assignment asks for MySQL, so use MySQL + `schema.sql` for the final submission.
