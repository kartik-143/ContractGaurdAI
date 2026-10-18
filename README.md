# ContractGuard AI — Hackathon MVP

A runnable contract-review prototype featuring PDF text extraction, clause segmentation, a bundled retrieval knowledge base, heuristic risk checks, suggested wording, a multi-agent-style workflow trace, SQLite review history, and JSON report export. An optional OpenAI API key adds an LLM-generated executive summary and next steps.

> **Important:** This is a hackathon prototype, not legal advice or a verified compliance system. The bundled knowledge base contains general drafting checklists, not a comprehensive or validated database of laws. Always verify sources and findings with a qualified reviewer. Do not upload confidential contracts to an untrusted deployment.

## Features

- Drag-and-drop PDF upload (up to 15 MB)
- PDF text extraction using pypdf
- Clause candidate segmentation
- Local retrieval against eight bundled drafting checklists
- Rule-based potential-risk flags for payment, termination, liability, confidentiality, data protection, renewal, IP, and dispute resolution
- Checks for potentially missing termination, liability, and dispute-resolution clauses
- Suggested revisions and evidence cards
- Agent workflow trace (Document, Research, Risk, Recommendation)
- Risk indicator, executive summary, review checklist, severity filtering
- SQLite review history and JSON report download
- Optional OpenAI-generated summary if `OPENAI_API_KEY` is configured
- Responsive dashboard UI

## Run on Windows

1. Install Python 3.11 or newer from https://www.python.org/downloads/ and enable **Add Python to PATH**.
2. Extract this project folder.
3. Open Command Prompt inside the `contractguard-ai` folder.
4. Run:

```bat
python -m venv .venv
.venv\Scripts\activate
python -m pip install --upgrade pip
pip install -r requirements.txt
uvicorn app.main:app --reload
```

5. Open http://127.0.0.1:8000 in your browser. Do not open `static/index.html` directly as a file; the app must be served by FastAPI.

## Saving and troubleshooting

- Analysis reports are stored in `contractguard.db` in this project folder. Keep using the same extracted folder to see previous reviews.
- The `/api/health` endpoint reports `saved_analyses` and the database file path.
- After analyzing a PDF, open `http://127.0.0.1:8000/api/history` to confirm the report appears in the `items` list.
- If you have multiple copies of the project, stop the old server with `Ctrl+C`, open Command Prompt in the folder you intend to use, and start that copy. The terminal path should end in `contractguard-ai`.
- Do not delete `contractguard.db` if you want to keep saved reports. Moving the project folder also moves its database.

## Optional LLM summary

Set an environment variable before starting the server. In Windows Command Prompt:

```bat
set OPENAI_API_KEY=your_api_key_here
set OPENAI_MODEL=gpt-4o-mini
uvicorn app.main:app --reload
```

The API key is optional; the app works in local heuristic mode without it. Never commit a real API key or include it in a public repository. LLM enhancement currently produces an executive summary and priority actions; the rule-based findings remain the primary detection mechanism.

## API

- `GET /api/health` — health and mode
- `GET /api/knowledge-base` — bundled source metadata
- `GET /api/history` — recent reports
- `POST /api/analyze` — multipart PDF analysis
- `GET /api/report/{report_id}` — report JSON
- `GET /api/report/{report_id}/download` — download JSON report

## Demo flow

1. Upload a text-based sample PDF.
2. Wait for analysis to finish.
3. Open high-priority findings and expand them.
4. Show the clause excerpt, explanation, retrieved drafting guidance, suggested revision, and agent trace.
5. Export the JSON report and revisit it from Review History.

## Known limitations

- Scanned PDFs need OCR; OCR is not included.
- Clause segmentation is heuristic and may not preserve exact legal clause boundaries.
- The bundled references are drafting checklists; URLs are source portals, not direct citations to a specific legal provision.
- Risk indicator is a heuristic demonstration metric, not a validated legal risk score.
- The prototype has no authentication, multi-user access control, production document retention policy, or hardened deployment configuration.


## India-focused knowledge sources
The bundled reference links now point to Indian government portals: India Code (Ministry of Law and Justice), MeitY data protection framework, and IP India. The bundled passages are drafting checklists, not legal statutes or verified legal advice. Check the current legislation, rules, applicability, and exact provisions before relying on any result.
