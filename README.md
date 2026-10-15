# AI-Powered Test Case Generator

Generate structured software test cases from a plain-language requirement using a React frontend, a FastAPI backend, and the Groq API.

The application generates 5–10 distinct positive and negative test cases with preconditions, actionable steps, expected results, and priority. Generated cases are saved to a local SQLite database and can be reviewed later or exported as CSV, JSON, or Excel.

## Features

- AI-generated, schema-validated test cases
- Positive and negative test coverage
- Generation history stored locally in SQLite
- Export to CSV, JSON, and XLSX
- Responsive React interface with light and dark themes
- FastAPI health check and interactive API documentation

## Requirements

- Python 3.10 or newer
- Node.js and npm
- A Groq API key

## Setup

### 1. Configure the backend

From the repository root, create a Python virtual environment and install the backend dependencies:

```bash
cd backend
python -m venv .venv
```

Activate it on Windows:

```powershell
.venv\Scripts\Activate.ps1
```

Or on macOS/Linux:

```bash
source .venv/bin/activate
```

Install dependencies:

```bash
python -m pip install -r requirements.txt
```

Create `backend/.env` and add your Groq API key:

```dotenv
GROQ_API_KEY=your_groq_api_key
```

Do not commit `.env` or publish your API key.

Start the backend from the `backend` directory:

```bash
uvicorn main:app --reload
```

The API is available at <http://127.0.0.1:8000>. Interactive documentation is available at <http://127.0.0.1:8000/docs>, and the health check is <http://127.0.0.1:8000/health>.

### 2. Configure the frontend

In a second terminal, from the repository root:

```bash
cd frontend
npm ci
npm run dev
```

Open the local URL printed by Vite (typically <http://127.0.0.1:5173>).

The frontend uses `http://127.0.0.1:8000` as its default backend URL. To use another API URL, create `frontend/.env` based on `frontend/.env.example` and set:

```dotenv
VITE_API_BASE_URL=http://127.0.0.1:8000
```

Restart the Vite development server after changing environment variables.

## API overview

| Method | Endpoint | Description |
| --- | --- | --- |
| `GET` | `/health` | Check backend health |
| `POST` | `/api/generate-test-cases` | Generate test cases from a requirement |
| `GET` | `/api/history` | List saved generations |
| `GET` | `/api/history/{generation_id}` | Retrieve a saved generation |
| `DELETE` | `/api/history/{generation_id}` | Delete a saved generation |
| `GET` | `/api/export/{generation_id}?format=csv` | Export as CSV, JSON, or XLSX |

Example generation request:

```json
{
  "requirement": "Users can reset their password using a verified email address."
}
```

## Development checks

Run the backend tests from the `backend` directory with the virtual environment activated:

```bash
pytest
```

Create a production frontend build from the `frontend` directory:

```bash
npm run build
```

## Project structure

```text
backend/
  main.py             FastAPI routes, SQLite history, and exports
  llm_service.py      Groq test-case generation and validation
  schemas.py          Request and response models
  requirements.txt    Python dependencies
  test_main.py        Backend API tests
frontend/
  src/
    components/       Shared interface components
    hooks/            React hooks
    pages/            Home, generation, history, and information pages
    utils/             Validation, formatting, and error helpers
  package.json        Frontend scripts and dependencies
```

## Data and privacy

Generation history is stored in `backend/test_cases.db`, created automatically when the API starts. Keep your API key private, and review the provider's terms before sending requirements that contain sensitive information.
