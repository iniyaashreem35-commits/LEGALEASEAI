# LegalEaseAI

## AI-Powered Legal Document Generator

**Team ID:** SWTID-2026-2057
**Team Size:** 4
**Team Leader:** Iniyaashree M
**Team Members:** Kaniga P, Kanimozhi S, Kanimozhi G

> **Important:** LegalEaseAI creates AI-assisted drafts for information and editing. It is not a substitute for legal advice, does not guarantee legal validity, and should be reviewed by a qualified legal professional before signing, filing, or relying on the document.

## Project Overview

LegalEaseAI is an AI-assisted legal document drafting application built with **FastAPI**, **Streamlit**, **Python**, and the **Google Gemini API**.

The application accepts a document type, parties involved, terms and conditions, and an effective date. It generates an editable first draft that can be downloaded as **TXT, DOCX, or PDF**.

## Features

- Preset and custom legal document types

- Parties, terms, and effective-date input

- AI document generation using Google Gemini

- Editable document preview in the browser

- TXT, DOCX, and PDF downloads

- Optional PNG, JPG, or JPEG logo upload

- FastAPI root and health endpoints

- Pydantic request validation

- Retry handling for temporary Gemini failures

- Environment-based API-key configuration

- Legal disclaimer in the interface and generated-document footer

## Technology Stack

| Technology | Purpose |
| --- | --- |
| Python | Application runtime |
| FastAPI | Backend REST API |
| Uvicorn | ASGI server |
| Streamlit | Browser user interface |
| Google GenAI SDK | Gemini API integration |
| Pydantic | Request validation |
| Requests | Frontend-to-backend communication |
| python-dotenv | Environment configuration |
| python-docx | DOCX generation |
| fpdf2 | PDF generation |
| Pillow | Logo/image handling |

## Project Structure

```
LegalEaseAI/
├── ai_core/
│   ├── __init__.py
│   └── gemini_generator.py
├── utils/
│   ├── __init__.py
│   └── document_formatter.py
├── app.py
├── main.py
├── routes.py
├── requirements.txt
├── .env
└── .gitignore
```

## Prerequisites

- Python 3.10 or newer

- Google Gemini API key for live generation

- Internet connection for Gemini API requests

- Modern web browser

## Installation

### 1. Open the project folder

```bash
cd LegalEaseAI
```

### 2. Create a virtual environment

**Windows PowerShell:**

```
python -m venv .venv
.venv\Scripts\Activate.ps1
```

**Windows Command Prompt:**

```
python -m venv .venv
.venv\Scripts\activate
```

**Linux or macOS:**

```bash
python3 -m venv .venv
source .venv/bin/activate
```

### 3. Install dependencies

```bash
pip install -r requirements.txt
```

## Configuration

Create a `.env` file in the project root:

```
GEMINI_API_KEY=your_gemini_api_key_here
GEMINI_MODEL=gemini-3.5-flash-lite
BACKEND_URL=http://127.0.0.1:8000
```

**Never commit ****`.env`**** or expose your Gemini API key.**

| Variable | Description |
| --- | --- |
| `GEMINI_API_KEY` | Secret key for Google Gemini; required for live generation |
| `GEMINI_MODEL` | Optional Gemini model name |
| `BACKEND_URL` | FastAPI URL used by Streamlit |

## Running the Application

LegalEaseAI uses two processes.

### Start the FastAPI backend

```bash
python -m uvicorn main:app --reload --host 127.0.0.1 --port 8000
```

Backend URLs:

- [http://127.0.0.1:8000/](http://127.0.0.1:8000/)

- [http://127.0.0.1:8000/health](http://127.0.0.1:8000/health)

- [http://127.0.0.1:8000/docs](http://127.0.0.1:8000/docs)

### Start the Streamlit frontend

Open a second terminal, activate the virtual environment, and run:

```bash
streamlit run app.py
```

The frontend normally opens at:

```
http://localhost:8501
```

## How to Use

1. Open the Streamlit URL.

1. Read the legal disclaimer.

1. Select a document type or enter a custom type.

1. Enter the effective date.

1. Enter the parties involved.

1. Enter the terms and conditions.

1. Optionally upload a logo.

1. Click **Generate Document**.

1. Review and edit the generated draft.

1. Download the result as TXT, DOCX, or PDF.

1. Obtain qualified legal review before using the document.

## API Reference

### `GET /`

Confirms that the API is running.

Example response:

```json
{
  "message": "LegalEase API is running",
  "status": "success"
}
```

### `GET /health`

Checks backend health.

```json
{
  "status": "healthy"
}
```

### `POST /generate`

Generates an AI-assisted legal document draft.

#### Request body

```json
{
  "document_type": "Freelance Work Contract",
  "parties": "Jane Doe (Service Provider ), Example Corp (Client)",
  "terms": "Payment within 30 days; delivery by deadline; confidentiality; 15 days notice for termination.",
  "effective_date": "24/09/2026"
}
```

#### Successful response

```json
{
  "document": "FREELANCE WORK CONTRACT\n\n1. PARTIES\n..."
}
```

### Input Limits

| Field | Minimum | Maximum |
| --- | --- | --- |
| `document_type` | 2 characters | 100 characters |
| `parties` | 2 characters | 5,000 characters |
| `terms` | 2 characters | 10,000 characters |
| `effective_date` | 2 characters | 100 characters |

## Gemini Prompt Safeguards

The generator instructs Gemini to:

- Use only information supplied by the user

- Avoid inventing names, addresses, dates, amounts, laws, courts, registration numbers, or other facts

- Use `[TO BE COMPLETED]` when important information is missing

- Use clear professional language

- Include appropriate sections and signature areas

- Avoid claiming that the document is legally valid

- Avoid providing legal advice

- Return plain text without Markdown code fences

## Error Handling

The application handles:

- Empty required fields

- Invalid field lengths

- Missing Gemini API key

- Empty Gemini responses

- Temporary Gemini 503, unavailable, 429, and quota errors

- Backend connection failures

- Non-success backend responses

- Document formatting errors

Temporary Gemini failures are retried with increasing wait times.

## Security Guidelines

- Never commit `.env` or expose `GEMINI_API_KEY`.

- Use a managed secret store in production.

- Restrict CORS origins before public deployment.

- Add authentication and rate limiting before public exposure.

- Avoid logging confidential legal document content unnecessarily.

- Define data retention and deletion policies before adding storage.

- Review uploaded-logo file handling and file-size limits.

## Testing Checklist

- Verify `GET /` returns a success response.

- Verify `GET /health` returns healthy status.

- Verify valid `POST /generate` returns a `document` field.

- Verify blank and oversized fields are rejected.

- Verify missing Gemini configuration shows a clear error.

- Verify temporary Gemini failures are handled gracefully.

- Verify backend-unavailable errors appear in Streamlit.

- Verify edited text is included in downloaded files.

- Verify TXT, DOCX, and PDF files open successfully.

- Verify special HTML characters are escaped in the preview.

- Verify the legal disclaimer remains visible.

Structural success does not prove legal correctness. Generated documents must be reviewed by a qualified legal professional.

## Limitations

- LegalEaseAI does not provide legal advice.

- It does not guarantee legal validity or completeness.

- It does not perform jurisdiction-specific legal analysis.

- It does not verify every fact generated by Gemini.

- It does not provide accounts or saved document history.

- It does not provide attorney approval or court filing.

- Live generation depends on Gemini availability, quota, and network access.

## Future Enhancements

- Reusable templates and clause libraries

- User accounts and encrypted document history

- Document version comparison

- Jurisdiction and language profiles

- Rate limiting, analytics, and cost controls

- Accessibility improvements

- Human legal-review workflow

- Citation and source-support features

- Automated content-quality benchmarks

## License and Usage

Add the project license before public distribution. Until then, treat LegalEaseAI as an academic or internal prototype.

## Project Team

- **Team ID:** SWTID-2026-2057

- **Team Leader:** Iniyaashree M

- **Team Members:** Kaniga P, Kanimozhi S, Kanimozhi G
