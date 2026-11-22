# Human API — Expertise Discovery Prototype

A small learning prototype that suggests which employee is best suited to a task, based on their skills, past projects and completed work — and explains why. The final decision always stays with a person; the system only recommends.

> Portfolio project by **Abdelrahman Dabash**, based on ideas explored in a university software engineering course (SWE 401). This repository is a personal learning version, not the graded team submission.

## The problem
Teams often pick people for a task using job titles, CVs or memory. Those don't always show what someone has actually done. Human API looks at recorded evidence of past work instead, and ranks candidates for a task written in plain language.

## How it works
1. A user enters a task title and a description in their own words.
2. The backend splits the text into words and compares them with each employee's skills, project summaries and completed tasks.
3. Employees are ranked by the share of task words found in their evidence.
4. Each result shows the matched words, where the evidence came from, an evidence-strength label (`insufficient`, `limited`, `reasonable`) and a short explanation.

The score is a transparent keyword indicator, not an AI model and not proof of ability.

## Tech stack
- **Backend:** Python, FastAPI, Pydantic
- **Frontend:** React, Vite
- **Tests:** pytest with FastAPI's TestClient
- **Data:** fictional in-memory sample records (no database yet)

## API endpoints
| Method | Path | What it does |
|---|---|---|
| GET | `/health` | Service status |
| GET | `/employees` | List sample employees |
| GET | `/employees/{id}` | One employee (404 if not found) |
| POST | `/matches` | Rank employees for `{title, description}` |
| GET | `/matches/{request_id}` | Fetch an earlier result |

Interactive docs are available at `http://127.0.0.1:8000/docs` while the backend runs.

## Run it locally

**Requirements:** Python 3.10+, Node.js 18+

### Backend
```powershell
cd backend
python -m venv .venv
.venv\Scripts\Activate.ps1
pip install -r requirements.txt
uvicorn main:app --reload
```

### Frontend (second terminal)
```powershell
cd frontend
npm install
npm run dev
```
Open the address Vite prints (usually `http://localhost:5173`).

## Tests
```powershell
cd backend
pytest -q
```
The tests cover the health check, listing employees, ranked matching with explanations, rejecting an empty title, and the 404 for an unknown employee.

## Known limitations
- Keyword matching only: no synonyms, no stemming, and common words count as matches.
- Employees with no matching evidence are still listed with a 0% score.
- No login, roles or persistent database; data resets when the server restarts.
- The frontend calls the backend on a fixed address (`127.0.0.1:8000`) and needs CORS enabled on the backend to work in the browser.

## Ideas for next steps
- Add CORS, a stop-word list and better tokenizing
- Show a clear "no match" message instead of 0% candidates
- Store data in PostgreSQL and add sign-in with roles
- Add tests that check the *right* person ranks first

## Privacy
All employee records are fictional. Never add real personal data, passwords or API keys to this repository.
