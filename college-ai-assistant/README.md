# KIET CollegeAI Assistant

## 1. Project Overview
KIET CollegeAI Assistant is a hybrid AI-powered chatbot for KIET Group of Institutions, Delhi-NCR, Ghaziabad.
It is designed only for B.Tech CSE/CS students (Even Semester 2025-26, 3rd and 4th semester scope).

Hybrid architecture:
- Layer 1: Structured JSON lookup (fast and accurate answers)
- Layer 2: FAISS + Groq RAG fallback (semantic retrieval for complex queries)

## 2. Tech Stack

| Layer | Technology |
|---|---|
| Backend API | FastAPI (Python) |
| Structured Engine | Custom JSON loader/search |
| RAG Framework | LangChain + FAISS |
| Embeddings | sentence-transformers (all-MiniLM-L6-v2) |
| LLM | Groq API (llama3-8b-8192) |
| PDF Extraction | pdfplumber + PyMuPDF |
| Frontend | HTML + CSS + Vanilla JavaScript |
| Storage | Local vector store (FAISS index on disk) |

## 3. Prerequisites
- Python 3.9+
- pip
- Git

## 4. Run Locally

### a) Clone the repository
```bash
git clone <your-repo-url>
cd college-ai-assistant
```

### b) Create and activate a virtual environment (recommended)
```bash
python -m venv .venv
source .venv/bin/activate
```

If you are using Git Bash on Windows, this also works:
```bash
source .venv/Scripts/activate
```

### c) Install dependencies
```bash
pip install -r backend/requirements.txt
```

### d) Get a Groq API key (optional, but recommended)
- Visit https://console.groq.com
- Create/login account
- Generate API key

### e) Configure environment
Create or edit `.env` in project root:
```env
GROQ_API_KEY=your_key_here
```

If `.env` is missing, the app can still answer many structured JSON-based queries, but Groq-backed fallback responses will not work.

### f) Confirm data files exist
Copy your JSON files into:
- `data/structured/`

Copy your PDFs into:
- `data/pdfs/`

### g) Build the vector index
```bash
python backend/ingest.py
```

### h) Start the backend API
```bash
python -m uvicorn backend.main:app --host 127.0.0.1 --port 8000 --reload
```

Backend URL:
- `http://127.0.0.1:8000`
- Health check: `http://127.0.0.1:8000/health`

### i) Start the frontend locally
Open a second terminal and run:
```bash
cd frontend
python -m http.server 5500 --bind 127.0.0.1
```

Frontend URL:
- `http://127.0.0.1:5500`

The frontend calls the backend at `http://localhost:8000` by default, so keep the FastAPI server running on port `8000` unless you also update the frontend API base.

### j) Local startup summary
Terminal 1:
```bash
python -m uvicorn backend.main:app --host 127.0.0.1 --port 8000 --reload
```

Terminal 2:
```bash
cd frontend
python -m http.server 5500 --bind 127.0.0.1
```

Then open:
```bash
http://127.0.0.1:5500
```

## 5. Folder Structure

```text
college-ai-assistant/
├── backend/
│   ├── calculator.py
│   ├── chat_handler.py
│   ├── config.py
│   ├── ingest.py
│   ├── json_loader.py
│   ├── llm_handler.py
│   ├── main.py
│   ├── retriever.py
│   └── requirements.txt
├── data/
│   ├── pdfs/
│   └── structured/
├── frontend/
│   ├── app.js
│   ├── index.html
│   └── style.css
├── vector_store/
├── .env
├── .gitignore
└── README.md
```

## 6. Example Questions
- When is MSE1?
- Show exam schedule for ESE.
- When is Holi holiday?
- Subjects in 4th sem?
- Syllabus of CS401L.
- Marks for IT302L.
- What is attendance policy?
- How many classes do I need to attend for 75%?
- Professional elective options in 4th sem.

## 7. Troubleshooting

### Backend not reachable from frontend
- Ensure FastAPI is running at `http://127.0.0.1:8000`
- Ensure the frontend is being served from `http://127.0.0.1:5500`
- Check terminal logs for startup errors

### GROQ key errors
- Verify `.env` contains valid `GROQ_API_KEY`
- Restart API server after updating `.env`

### JSON answers not coming
- Confirm files exist in `data/structured/`
- Confirm JSON is valid (no syntax errors)
- Check `/health` to verify `json_files_loaded`

### Vector store not loading
- Run `python backend/ingest.py`
- Ensure `vector_store/faiss_index` exists and contains `index.faiss` and `index.pkl`

### Port already in use
- If port `8000` is busy, stop the other process or run the backend on a different port
- If you change the backend port, update the frontend API base accordingly before testing locally

### PDF extraction issues
- Prefer selectable-text PDFs over scanned image PDFs
- Re-run ingestion after replacing files

## 8. Branch Note
Designed for KIET CSE/CS branch only.
