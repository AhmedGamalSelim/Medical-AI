Medical Lab AI V7
V7 extends V6 with an end-to-end persistent API flow for synthetic reports.
Flow: Authenticated User -> Synthetic Report -> Processing -> LabResult + ProcessingStep + AuditEvent -> Results -> Human Review
The identity bridge in this development build uses X-User-ID and is not production authentication. Replace it with the project's JWT authentication before production deployment.
Current safety behavior
No diagnosis from labs alone.
Evidence must reference actual extracted results.
Conflicts and low OCR confidence require review.
No medication-change recommendations.
Synthetic data only.
V8 hardening
Added E2E API contract tests.
Added production authentication migration requirements.
Added aiosqlite for isolated async integration-test environments.
The development identity bridge remains explicitly non-production.
V9 fixes
Fixed pytest import-path failure.
Added configurable database URL instead of hardcoded SQLite.
Added JWT register/login and bearer authentication.
Kept X-User-ID only as a development fallback; disabled in production.
Added required PostgreSQL/Redis/Celery/JWT dependencies.
Added minimal Celery worker module so Docker Compose does not reference a missing app.
Fixed synthetic dataset path resolution for local and container execution.
V11 hardening
Fixed local synthetic dataset resolution in Pipeline.
Added direct pipeline execution regression tests.
Added HTTP integration test contract for JWT and ownership.
Integration tests use SQLite/aiosqlite and are skipped only when that dependency is absent.
V14 OCR
The OCR layer uses Tesseract with English + Arabic language packs, PyMuPDF for PDF rendering, deterministic image preprocessing, token confidence, page metadata, and bounding boxes. OCR does not itself diagnose or interpret results; low-confidence OCR is routed to review.
V23 hardening
V23 fixes image-validation imports, hardens Outbox dispatch with stale DISPATCHING recovery and retry backoff, corrects missing synthetic confidence defaults, and updates the test contract to the Outbox architecture.
V35 Knowledge Base
Clinical rules are now loaded from a versioned knowledge-base registry. A rule is not production-ready unless it is explicitly ACTIVE and has registered source IDs. The current synthetic microcytic rule remains PROVISIONAL_TEST_ONLY and must not be treated as validated clinical guidance.
V91 Analysis Integrity
Every pipeline analysis now includes deterministic analysis_integrity metadata with a schema version, SHA-256 fingerprint, result count, and evidence count. The fingerprint is an audit/reconciliation aid and excludes patient context and raw OCR text.
V150 final release
The current software-hardening checkpoint is V150 (application version 1.50.0). Release readiness is gated by automated contracts and build checks. This does not constitute clinical validation or authorization for autonomous diagnosis or treatment.
Quick Start V183
See QUICKSTART_AR.md. On Windows run tools\start-windows.bat; on Linux run ./tools/start-linux.sh.
