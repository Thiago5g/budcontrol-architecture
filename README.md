<p align="center">
  <h1 align="center">BudControl — AI-Powered Personal Finance Platform</h1>
</p>

<p align="center">
  <img src="https://img.shields.io/badge/NestJS-11-E0234E?style=for-the-badge&logo=nestjs&logoColor=white" />
  <img src="https://img.shields.io/badge/React-19-61DAFB?style=for-the-badge&logo=react&logoColor=black" />
  <img src="https://img.shields.io/badge/Flutter-3.7+-02569B?style=for-the-badge&logo=flutter&logoColor=white" />
  <img src="https://img.shields.io/badge/TypeScript-5-3178C6?style=for-the-badge&logo=typescript&logoColor=white" />
  <img src="https://img.shields.io/badge/AWS-ECS%20%7C%20S3%20%7C%20RDS-232F3E?style=for-the-badge&logo=amazonaws&logoColor=white" />
  <img src="https://img.shields.io/badge/OpenAI-GPT%20%7C%20Vision%20%7C%20Whisper-412991?style=for-the-badge&logo=openai&logoColor=white" />
  <img src="https://img.shields.io/badge/PostgreSQL-16-4169E1?style=for-the-badge&logo=postgresql&logoColor=white" />
</p>

<p align="center">
  <strong>Production SaaS • Real Users • Real Transactions</strong><br/>
  A multi-service platform combining conversational AI, document intelligence, and native mobile — built and operated as a solo founder.
</p>

---

## Overview

BudControl is a personal finance management platform with an AI-powered assistant that lives where users already are — WhatsApp. Users photograph receipts, forward invoices, send audio messages, and the system extracts, categorizes, and tracks their finances automatically.

The platform is **live in production** serving real users with real financial data:

- **Web Panel & PWA:** [painel.budcontrol.app](https://painel.budcontrol.app)
- **REST API:** [api.budcontrol.app](https://api.budcontrol.app)
- **Mobile:** iOS & Android (Flutter container)

The system processes credit card invoices, payment receipts, bank statements, and free-form expense descriptions — all through natural conversation or direct upload.

---

## System Architecture

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                              CLIENTS                                         │
├──────────────────┬──────────────────────┬───────────────────────────────────┤
│                  │                      │                                   │
│  ┌────────────┐  │  ┌────────────────┐  │  ┌─────────────────────────────┐  │
│  │  budfront  │  │  │  Flutter App   │  │  │      WhatsApp (Meta)        │  │
│  │  React 19  │  │  │  WebView +     │  │  │      33+ AI Tools           │  │
│  │  Vite/PWA  │  │  │  Native Bridge │  │  │      Multi-modal Input      │  │
│  └─────┬──────┘  │  └───────┬────────┘  │  └──────────────┬──────────────┘  │
│        │         │          │           │                 │                  │
└────────┼─────────┴──────────┼───────────┴─────────────────┼──────────────────┘
         │                    │                             │
         │    JS ↔ Dart       │         Meta Webhook        │
         │    Native Bridge   │                             │
         ▼                    ▼                             ▼
┌─────────────────────────────────────────────────────────────────────────────┐
│                        bud_controll_api (NestJS 11)                          │
│                                                                             │
│  ┌─────────┐  ┌──────────┐  ┌────────────┐  ┌───────────┐  ┌───────────┐  │
│  │  Auth   │  │ Finance  │  │  WhatsApp  │  │    AI     │  │   Push    │  │
│  │  JWT    │  │  CRUD    │  │  Pipeline  │  │  Engine   │  │  FCM/Web  │  │
│  └─────────┘  └──────────┘  └────────────┘  └───────────┘  └───────────┘  │
└────────┬────────────────┬─────────────────────────┬─────────────────────────┘
         │                │                         │
         ▼                ▼                         ▼
┌──────────────┐  ┌──────────────┐  ┌──────────────────────────────────────┐
│  PostgreSQL  │  │   AWS S3     │  │           OpenAI                     │
│  (AWS RDS)   │  │  Invoices    │  │  GPT-4 • Vision • Whisper • Embeddings│
│              │  │  Staging+Perm│  │                                      │
└──────────────┘  └──────────────┘  └──────────────────────────────────────┘
```

---

## AI & Intelligence Layer

### WhatsApp Conversational Assistant

The core AI experience is a WhatsApp chatbot with **33+ specialized tools** that acts as a personal finance assistant. Users interact naturally — sending text, audio, photos, and documents.

**Processing Pipeline:**

```
Meta Webhook
     │
     ▼
FileTypeDetector ──→ ContentExtractor ──→ DocumentClassifier (AI)
                                                   │
                          ┌────────────────────────┼────────────────────┐
                          ▼                        ▼                    ▼
                   ReceiptHandler        InvoiceHandler        StatementHandler
                          │                        │                    │
                          └────────────────────────┼────────────────────┘
                                                   ▼
                                            Preview Message
                                                   │
                                                   ▼
                                         User Confirmation
                                                   │
                                                   ▼
                                              Persist Data
```

**Supported Input Types:**
- Payment receipts (OCR via OpenAI Vision)
- Credit card invoices (PDF parsing + AI extraction)
- Bank statements (CSV, OFX, PDF)
- Expense lists (natural language → structured data)
- Audio messages (Whisper transcription → intent resolution)

### Document Intelligence

| Input | Processing | Output |
|-------|-----------|--------|
| Photo of receipt | Vision OCR → AI extraction | Pre-filled transaction |
| PDF invoice | Text extraction → AI parsing | Itemized expenses with due dates |
| Bank statement | Format detection → row parsing | Batch transactions with dedup |
| Audio message | Whisper → NLU → tool routing | Executed finance action |

### Smart Categorization

A hybrid approach combining AI inference with user-defined rules. The system learns from corrections and applies consistent categorization across import sources.

---

## Infrastructure

| Component | Service | Purpose |
|-----------|---------|---------|
| **Compute** | AWS ECS (Fargate) | Containerized API, auto-scaling |
| **Database** | AWS RDS PostgreSQL | Financial data, user accounts, transaction history |
| **Storage** | AWS S3 | Invoice PDFs, receipt images (staging + permanent buckets) |
| **Mobile CI/CD** | Codemagic | Automated iOS/Android builds and store deployment |
| **Push (Native)** | Firebase Cloud Messaging | iOS & Android push notifications |
| **Push (Web)** | VAPID / Web Push | Browser and PWA notifications |
| **AI** | OpenAI API | GPT-4, Vision, Whisper, function calling |
| **Messaging** | Meta WhatsApp Business API | Chatbot webhook + message delivery |

---

## Key Engineering Decisions

**1. Preview-Before-Persist Pattern**
Every AI extraction is shown to the user for review before persisting. This eliminates silent data corruption from AI hallucinations while maintaining a smooth UX. The only exception is email invoice imports, which follow a batch-confirm flow.

**2. S3 Two-Tier Storage with TTL**
Uploaded files land in a staging bucket with a 24-hour TTL. Only upon user confirmation are they promoted to permanent storage. This prevents orphaned files from abandoned flows without requiring garbage collection jobs.

**3. Flutter WebView Architecture**
Instead of building native UI twice, the React PWA handles all interface rendering. Flutter provides the native shell — biometrics (`local_auth`), push tokens (FCM), clipboard access, and deep links to banking apps. A JS ↔ Dart bridge (`nativeBridge`) coordinates capabilities.

**4. App-Level Encryption for Payment Codes**
Brazilian payment codes (boleto barcodes, Pix keys) are encrypted at the application layer via `PaymentCodeCryptoService` before database storage. Even with database access, sensitive payment identifiers remain protected.

**5. Aggressive Deduplication**
Financial data arrives from multiple channels — WhatsApp photos, PDF imports, manual entry, email parsing. A deduplication engine cross-references amount, date, description, and source metadata to prevent duplicate transactions across all ingestion paths.

**6. Tool-Calling Architecture over Hard-Coded Flows**
Instead of building separate endpoints for each WhatsApp action, the system uses OpenAI function calling with a registry of 33+ tools. Adding a new financial action means defining a tool schema and handler — no changes to the conversation engine. This mirrors how AI labs build agentic systems.

**7. Multi-Channel Ingestion with Unified Dedup**
Financial data arrives from 5+ channels (WhatsApp photo, email PDF, manual entry, CSV import, OFX import). A deduplication engine cross-references amount, date, description similarity, and source metadata to prevent duplicates without blocking legitimate similar transactions.

---

## AI Tool Architecture

The WhatsApp assistant uses an **agentic tool-calling pattern** with 33+ specialized tools:

```
┌──────────────┐     ┌──────────────┐     ┌──────────────┐
│ User Message │────▶│  GPT-4 with  │────▶│ Tool Router  │
│ (text/audio) │     │  Tool Defs   │     │              │
└──────────────┘     └──────────────┘     └──────┬───────┘
                                                  │
          ┌───────────────────────────────────────┼──────────────────┐
          ▼                    ▼                   ▼                  ▼
  ┌──────────────┐   ┌──────────────┐  ┌──────────────────┐  ┌───────────┐
  │ Financial    │   │ Card/Invoice │  │ Accounts Payable │  │ Reports & │
  │ Movements    │   │ Management   │  │ & Payments       │  │ Analytics │
  └──────────────┘   └──────────────┘  └──────────────────┘  └───────────┘
```

**Each tool:**
- Has a typed input/output schema (validated before execution)
- Returns structured results for the LLM to format as natural language
- Logs execution with correlation ID for audit trail
- Respects confirmation rules — never mutates financial data without user consent
- Can be composed (one tool's output triggers another tool suggestion)

**Tool categories:**
| Category | Examples | Count |
|----------|----------|-------|
| Financial movements | Create, list, categorize, delete | 8 |
| Credit cards & invoices | Import, list charges, manage cards | 7 |
| Accounts payable | Create, pay, attach receipt, list due | 6 |
| Bank accounts | List, set default, check balance | 4 |
| Categories & rules | Create, list, set auto-categorization | 4 |
| Reports | Monthly summary, spending by category | 3 |
| System | Set intent, confirm batch, cancel | 3 |

---

## Distributed Processing & Reliability

```
┌─────────────┐     ┌─────────────┐     ┌──────────────┐     ┌─────────────┐
│ WhatsApp    │────▶│ File Type   │────▶│ Content      │────▶│ Classifier  │
│ Webhook     │     │ Detection   │     │ Extraction   │     │ (AI)        │
└─────────────┘     └─────────────┘     └──────────────┘     └──────┬──────┘
                                                                     │
                    ┌────────────────────────────────────────────────┘
                    ▼
            ┌──────────────┐     ┌──────────────┐     ┌──────────────┐
            │ Handler      │────▶│ Preview      │────▶│ Persist      │
            │ (per type)   │     │ + Confirm    │     │ (on confirm) │
            └──────────────┘     └──────────────┘     └──────────────┘
```

**Reliability patterns implemented:**

- **Idempotency:** Document processing uses content hashing to prevent duplicate imports across channels (email, WhatsApp, manual upload)
- **Staged persistence:** Files land in S3 staging (24h TTL) before promotion to permanent storage on user confirmation
- **Graceful degradation:** If OpenAI is unavailable, documents are queued for retry rather than failing the user interaction
- **Session state:** WhatsApp sessions track pending intents and batch confirmations, surviving individual message failures
- **Timeout handling:** AI extraction has configurable timeouts with fallback to manual input prompts
- **Atomic operations:** Financial mutations use database transactions to prevent partial state

---

## Observability

- **Structured logging:** JSON logs with correlation IDs per WhatsApp session, traceable across the full document pipeline
- **Error categorization:** Errors classified by type (AI extraction failure, payment code invalid, file too large) with context for debugging
- **Health monitoring:** API health endpoint with dependency checks (PostgreSQL, S3 connectivity, OpenAI reachability)
- **Classification confidence:** Document classifier outputs confidence scores, logged for quality monitoring and threshold tuning
- **Processing metrics:** Track per-document-type success rates, extraction accuracy, and retry counts

---

## Data Model Overview

```
User
 ├── BankAccounts
 ├── CreditCards
 │    └── CardExpenses
 │         └── Invoices (imported PDFs)
 ├── FinancialMovements (income/expense)
 ├── AccountsPayable
 │    ├── PaymentInstructions (encrypted)
 │    └── Attachments (S3 references)
 ├── Categories
 │    └── CategorizationRules
 └── WhatsappSessions
      └── PendingBatches (awaiting confirmation)
```

**Key relationships:**
- Every entity is scoped by `userId` (strict multi-tenancy)
- `PaymentInstructions` stores encrypted boleto/PIX codes (decrypted only at payment time)
- `PendingBatches` hold extracted data awaiting user confirmation before persisting as real records
- `CategorizationRules` are user-defined patterns that override AI suggestions for consistent categorization

---

## Tech Stack

| Layer | Technology | Details |
|-------|-----------|---------|
| **API** | NestJS 11, TypeORM, JWT | Modular architecture, resource-based modules |
| **Frontend** | React 19, Vite 6, TypeScript | Tailwind 4, Radix UI, Zustand state management |
| **Mobile** | Flutter 3.7+, WebView | Native bridge, FCM, biometrics, deep links |
| **Database** | PostgreSQL (RDS) | TypeORM migrations, multi-tenant row filtering |
| **Storage** | AWS S3 | Presigned URLs, TTL staging, permanent buckets |
| **Compute** | AWS ECS Fargate | Containerized deployment, auto-scaling |
| **AI/ML** | OpenAI GPT-4, Vision, Whisper | 33+ tool definitions, function calling, multi-modal |
| **Messaging** | Meta WhatsApp Business API | Webhook processing, session management |
| **Push** | Firebase (FCM) + Web Push (VAPID) | Cross-platform notification delivery |
| **CI/CD** | Codemagic | Automated mobile builds and store submission |

---

## Security & Multi-Tenancy

- **Strict tenant isolation:** every query is scoped by `userId` at the repository layer
- **JWT authentication** with guard-based route protection
- **Payment code encryption** at application level (not just at-rest DB encryption)
- **No cross-user data exposure** — enforced by architecture, not just access control
- **Sensitive operations require confirmation** — no silent financial data mutations

---

<p align="center">
  <em>This is a portfolio overview of a production system. The source code is in private repositories.</em>
</p>
