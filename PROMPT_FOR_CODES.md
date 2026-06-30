# Codex Task: Build Phase 1

Read `/docs/AI_Bookkeeping_Automation_Technical_Design_Document.md`.

Build Phase 1 only.

## Phase 1 Requirements

Create a local prototype web application that can:

1. Upload a Pizza Time Saloon Daily Sales PDF.
2. Extract the required fields from the report.
3. Apply the Pizza Time mapping rules.
4. Generate a draft journal entry.
5. Display total debits and credits.
6. Warn if the journal entry does not balance.
7. Allow export to CSV.

## Do Not Build Yet

- Do not connect to QuickBooks yet.
- Do not add multi-client support yet.
- Do not add user login yet.
- Do not deploy yet.

## Preferred Stack

- Frontend: React
- Backend: Python FastAPI
- PDF parsing: PyMuPDF
- Database: SQLite for prototype

## Deliverables

- Working app
- README with setup instructions
- Sample test using Pizza Time Saloon Daily Report
