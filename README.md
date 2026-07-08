

https://github.com/user-attachments/assets/affe90b8-1e73-4cb6-b187-ef6958658d40


# CodeAtlas

CodeAtlas is an interactive codebase analysis and visualization project that extracts structure and semantics from repositories, builds dependency/semantic graphs, and exposes AI-powered endpoints for exploring and explaining code.

**Quick Start**
- **Backend:** FastAPI service that provides ingestion, analysis and AI endpoints.
- **Frontend:** Vite + React UI for visualizing graphs and interacting with the AI features.

---

## Quick Setup

### Option 1: Local Development (No Docker)

**Backend (Windows):**
```bash
cd backend
python -m venv .venv
.\.venv\Scripts\activate
pip install -r requirements.txt
uvicorn app.main:app --reload --host 0.0.0.0 --port 8000
```

**Backend (macOS / Linux):**
```bash
cd backend
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
uvicorn app.main:app --reload --host 0.0.0.0 --port 8000
```

**Frontend:**
```bash
cd frontend
npm install
npm run dev
```

- Vite serves the UI at `http://localhost:5173`
- Backend runs at `http://localhost:8000`
- FastAPI interactive docs at `http://localhost:8000/docs`

---

### Option 2: Docker (All Platforms - Recommended)

```bash
# From project root
docker compose up --build -d

# Frontend: http://localhost:8080
# Backend:  http://localhost:8000
# Health:   http://localhost:8000/api/health
```

Stop with:
```bash
docker compose down
```

---

## Project Structure

```
codeatlas/
├── backend/                 # FastAPI application
│   ├── app/
│   │   ├── api/            # Routes & schemas
│   │   │   ├── routers/    # ai_chat, graph, ingest, job_store
│   │   │   └── schemas.py  # Pydantic models
│   │   └── services/       # analyzer, pipeline, repo_loader, job_store
│   ├── requirements.txt
│   ├── Dockerfile
│   ├── .dockerignore
│   └── .env.example
├── frontend/               # Vite + React + TypeScript
│   ├── src/
│   │   ├── components/     # GraphCanvas, Sidebar, DetailPanel, etc.
│   │   ├── hooks/          # useCodeAtlas
│   │   ├── api.ts          # Axios client (uses VITE_API_BASE_URL)
│   │   └── types.ts
│   ├── package.json
│   ├── vite.config.ts
│   ├── Dockerfile
│   ├── nginx.conf
│   ├── .dockerignore
│   └── .env.example
├── graphs/                 # Graph export utilities
├── ml/                     # ML experiments
├── sample-data/            # Demo documents
├── docker-compose.yml      # Local container orchestration
├── render.yaml             # Render.com Blueprint (backend)
├── vercel.json             # Vercel config (frontend)
├── FREE_TIER_DEPLOY.md     # Step-by-step free tier guide
└── README.md
```

---

## API Overview

The backend exposes endpoints under `/api/*`:

| Method | Endpoint | Description |
|--------|----------|-------------|
| `POST` | `/api/ingest/url` | Ingest by repository URL |
| `POST` | `/api/ingest/zip` | Ingest from uploaded zip |
| `POST` | `/api/ingest/paste` | Ingest pasted code/text |
| `GET` | `/api/ingest/status/{job_id}` | Check job status |
| `GET` | `/api/graph/{job_id}` | Fetch generated graph |
| `GET` | `/api/graph/{job_id}/summary` | Graph summary |
| `POST` | `/api/ai/explain` | AI node explanation |
| `POST` | `/api/ai/chat` | AI chat with codebase |
| `POST` | `/api/analyze/github` | Analyze GitHub repo (frontend) |

Example request:
```bash
curl -X POST http://localhost:8000/api/analyze/github \
  -H "Content-Type: application/json" \
  -d '{"repo_url":"https://github.com/your/repo.git","company_stack":[]}'
```

---

## Development Notes

- **FastAPI docs:** `/docs` (Swagger) and `/redoc`
- **CORS:** Allows `*` for development; configure `ALLOWED_ORIGINS` for production
- **Frontend API:** `src/api.ts` uses `VITE_API_BASE_URL` (defaults to `http://localhost:8000`)
- **Environment:** Copy `.env.example` → `.env` in both `backend/` and `frontend/`

---

## Deployment

### Free Tier (Recommended for Testing)

| Component | Platform | Config File | Free Tier Limits |
|-----------|----------|-------------|------------------|
| Backend (FastAPI) | Render | `render.yaml` | 750 hrs/mo, spins down after 15 min idle |
| Frontend (Vite+React) | Vercel | `vercel.json` | Unlimited personal, 100 GB bandwidth/mo |

**Step-by-step guide:** See [`FREE_TIER_DEPLOY.md`](FREE_TIER_DEPLOY.md)

**Quick Path:**
1. Push repo to GitHub
2. Deploy backend on Render using `render.yaml` (Root Directory: `backend/`)
3. Deploy frontend on Vercel using `vercel.json` (Root Directory: `frontend/`)
4. Set `VITE_API_BASE_URL` in Vercel to your Render backend URL
5. Update `ALLOWED_ORIGINS` in Render to your Vercel frontend URL

---

### Docker (Local / Self-Hosted / VPS)

```bash
# From project root
docker compose up --build -d

# Frontend: http://localhost:8080
# Backend:  http://localhost:8000
# Health:   http://localhost:8000/api/health
```

**Production Environment Variables:**

| Service | Variable | Production Value |
|---------|----------|------------------|
| Backend | `ALLOWED_ORIGINS` | `https://your-frontend.vercel.app` |
| Backend | `HF_TOKEN` | Your Hugging Face token (optional) |
| Backend | `SKIP_EMBEDDING` | `false` (if using HF embeddings) |
| Frontend | `VITE_API_BASE_URL` | `https://your-backend.onrender.com` |

---

### Deployment Files

| File | Purpose |
|------|---------|
| `render.yaml` | Render.com Blueprint for backend |
| `vercel.json` | Vercel configuration for frontend |
| `docker-compose.yml` | Local container orchestration |
| `backend/Dockerfile` | Backend container (multi-stage, non-root) |
| `frontend/Dockerfile` | Frontend container (nginx + static) |
| `frontend/nginx.conf` | Nginx config for SPA + API proxy |
| `backend/.dockerignore` | Optimize backend build context |
| `frontend/.dockerignore` | Optimize frontend build context |
| `FREE_TIER_DEPLOY.md` | Complete free tier deployment guide |

---

## Using Sample Data

See `sample-data/demo_documents.json` for example documents used by ingestion and demos.

---

## Contributing

- Fork and open a PR
- Add clear commit messages
- Update this README when adding features or endpoints

---

## License

See the `LICENSE` file at the repository root for licensing details.

---

## Need Help?

- **Free Tier Guide:** `FREE_TIER_DEPLOY.md`
- **Render Docs:** https://render.com/docs
- **Vercel Docs:** https://vercel.com/docs
- **Docker Compose:** https://docs.docker.com/compose/
- **Issues:** Open a GitHub issue in this repo