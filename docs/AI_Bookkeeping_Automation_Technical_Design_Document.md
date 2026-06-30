Technical Design Document AI Bookkeeping Automation Platform Initial
implementation: Pizza Time Saloon Daily Sales Journal Entry Automation
Version 1.0 \| Prepared for build with Codex

1.  Executive Summary The application converts client financial reports
    into reviewable journal entries. The first version supports Pizza
    Time Saloon daily sales reports and produces a draft QuickBooks
    Online journal entry using rules maintained in a mapping
    spreadsheet. QuickBooks posting is deliberately deferred until
    extraction and balancing are proven reliable. The system is designed
    as a multi-client platform. Adding new clients or report types
    should normally require adding a client profile, report template,
    extraction schema, and mapping rules---not rewriting the
    application.

2.  Scope In scope for Phase 1: local or hosted web app, PDF upload,
    Pizza Time extraction, mapping spreadsheet import, draft journal
    entry creation, balancing validation, CSV export, and review screen.
    Out of scope for Phase 1: live QuickBooks posting, payroll
    processing, automatic approval, bank reconciliation, and unattended
    production use.

3.  Design Principles Human approval before posting. Every generated
    entry must be reviewed before it reaches QuickBooks. Rules are data,
    not code. Client-specific accounting decisions should live in
    mapping tables or app configuration. Explainability. The review
    screen must show where each number came from and what rule produced
    each journal line. Safe failure. If a report does not match expected
    patterns or the journal entry does not balance, the system should
    stop and flag the exception.

4.  Target Architecture User uploads report PDF \| v PDF/Text Extraction
    Service \| v AI Extraction + Schema Validation \| v Mapping Rule
    Engine \| v Draft Journal Entry Builder \| v Review / Edit / Approve
    Screen \| v CSV Export in Phase 1; QuickBooks Online API in later
    phase

5.  Data Model

6.  Backend API Endpoints

7.  Pizza Time Daily Sales Extraction Schema

8.  Initial Mapping Rules - Pizza Time Daily Sales

9.  Formula Engine The formula engine evaluates safe expressions using
    extracted field names. It must not execute arbitrary code. Supported
    operations in Phase 1: addition, subtraction, constants, and
    optional zero defaults for missing inactive fields.

10. Validation Rules Journal entry debits must equal credits before
    approval. Each active rule must resolve to a QuickBooks account.
    Each extracted amount must be numeric and non-negative unless the
    field is explicitly allowed to be negative. Report date must be
    present. Duplicate detection should compare client, report type,
    report date, session number, and selected totals. Balancing
    differences should be flagged, not silently posted.

11. User Interface

12. Security and Permissions Require login for every user. Restrict
    client access by user role when staff are added. Store QuickBooks
    tokens encrypted when QuickBooks integration is added. Do not store
    OpenAI API keys or Intuit secrets in source code; use environment
    variables or a secrets manager. Log approvals and edits with user,
    timestamp, before values, and after values. Retain original uploaded
    reports for audit purposes.

13. QuickBooks Online Integration - Later Phase Phase 1 should export
    CSV only. QuickBooks Online posting should be added after the Pizza
    Time extraction and draft journal entries match manual entries
    across multiple test reports.

14. Test Plan

15. Suggested Repository Structure for Codex bookkeeping-automation/
    backend/ app/ main.py models.py schemas.py services/
    pdf_extractor.py ai_extractor.py mapping_engine.py
    journal_entry_builder.py csv_exporter.py tests/ requirements.txt
    frontend/ src/ pages/ components/ api/ package.json sample_data/
    pizza_time_daily_report.pdf mapping_rules.xlsx docs/
    technical_design_document.docx docker-compose.yml README.md

16. Codex Build Prompts Use these prompts one at a time. Do not ask
    Codex to build the entire production app in one step. Prompt 1:
    Create a FastAPI backend with endpoints for clients, report upload,
    extraction, draft journal entry creation, and CSV export. Use
    in-memory storage first so we can test quickly. Prompt 2: Add a
    Pizza Time extraction service that reads the uploaded PDF text and
    extracts values into the defined JSON schema. Include tests using
    the sample report. Prompt 3: Build a mapping engine that imports
    mapping_rules.xlsx and evaluates safe formulas against extracted
    fields. Add unit tests for Pizza Time mappings. Prompt 4: Create a
    journal entry builder that produces lines with account, debit,
    credit, description, source field, and rule ID. Add a balancing
    validation. Prompt 5: Create a React frontend with upload page,
    extracted values table, draft journal entry review table, totals,
    warnings, edit button, approve button, and CSV export button. Prompt
    6: Add PostgreSQL persistence and audit events after the in-memory
    prototype works.

17. Development Milestones

18. Open Questions Confirm whether cash deposit is always manually
    entered or should be calculated from Net Cash. Confirm how balancing
    differences should be handled when the report and bank deposit
    differ. Confirm exact QuickBooks account names and whether account
    numbers/classes/locations are used. Confirm whether employee meal
    treatment should always debit Personnel Costs:Employee Meals.
    Confirm whether paid outs always map to Entertainment or require
    categorization by memo/reason. Appendix A - Sample Journal Entry
    Output Shape Note: The above output shape is illustrative. The
    application must use the final approved mapping rules and balance
    check before posting or export.
