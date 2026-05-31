# CodeAtlas

CodeAtlas is an interactive codebase analysis and visualization project that extracts structure and semantics from repositories, builds dependency/semantic graphs, and exposes AI-powered endpoints for exploring and explaining code.

**Quick Start**
- **Backend:** FastAPI service that provides ingestion, analysis and AI endpoints.
- **Frontend:** Vite + React UI for visualizing graphs and interacting with the AI features.

**Quick Setup**

- Backend (Windows):

```bash
cd backend
python -m venv .venv
.\.venv\Scripts\activate
pip install -r requirements.txt
uvicorn app.main:app --reload --host 0.0.0.0 --port 8000
```

- Backend (macOS / Linux):

```bash
cd backend
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
uvicorn app.main:app --reload --host 0.0.0.0 --port 8000
```

- Frontend:

```bash
cd frontend
npm install
npm run dev
```

By default Vite serves the UI (usually at http://localhost:5173) and the backend runs at http://localhost:8000. FastAPI provides interactive docs at `/docs` and `/redoc` when the backend is running.

**Project Structure**
- **backend/**: FastAPI application and services.
	- **app/api/**: request/response schemas and routers (`ai_chat.py`, `graph.py`, `ingest.py`, `job_store.py`).
	- **app/services/**: analysis pipeline, repository loader and job store logic.
- **frontend/**: Vite + React UI (`src/` contains components, hooks, and styles).
- **graphs/**: utilities and export scripts for graph snapshots.
- **ml/**: machine learning related experiments and notes.
- **sample-data/**: example documents and demo inputs used for testing and demos.

**API Overview**
The backend exposes a number of endpoints under `/api/*`. Key routes available from the app root include:
- `POST /api/ingest/url` — ingest by repository URL.
- `POST /api/ingest/zip` — ingest from uploaded zip.
- `POST /api/ingest/paste` — ingest pasted code or text.
- `GET /api/ingest/status/{job_id}` — check ingest/analysis job status.
- `GET /api/graph/{job_id}` — fetch the generated graph data.
- `GET /api/graph/{job_id}/summary` — graph summary for quick overview.
- `POST /api/ai/explain` and `POST /api/ai/chat` — AI-assisted explanations and chat.
- `POST /api/analyze/github` — convenience endpoint for analyzing a GitHub repo (used by the frontend).

Example `curl` request to analyze a GitHub repository:

```bash
curl -X POST http://localhost:8000/api/analyze/github \
	-H "Content-Type: application/json" \
	-d '{"repo_url":"https://github.com/your/repo.git","company_stack":[]}'}
```

**Development Notes**
- FastAPI: interactive docs at `/docs` (Swagger UI).
- CORS: backend currently allows origins `*` for development.
- Frontend: adjust `src/api.ts` if your backend host/port differ from defaults.
- Add environment variables and secrets management before deploying to production.

**Using Sample Data**
- See `sample-data/demo_documents.json` for example documents used by ingestion and demos.

**Contributing**
- Fork and open a PR. Add clear commit messages and update this README when adding new features or endpoints.

**License**
- See the `LICENSE` file at the repository root for licensing details.

----
If you'd like, I can also:
- add run scripts or a `Makefile`/`docker-compose.yml` for easier local launches,
- generate example environment variable templates, or
- update the frontend README with developer tips. Which would you like next?
