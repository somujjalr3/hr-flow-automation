# HRFlow AI — Enterprise HR Workflow Automation Platform

[![FastAPI](https://img.shields.io/badge/FastAPI-0.110+-009688?logo=fastapi&logoColor=white)](https://fastapi.tiangolo.com)
[![React](https://img.shields.io/badge/React-19.3-61DAFB?logo=react&logoColor=black)](https://react.dev)
[![TypeScript](https://img.shields.io/badge/TypeScript-5.x-3178C6?logo=typescript&logoColor=white)](https://www.typescriptlang.org)
[![Tailwind CSS](https://img.shields.io/badge/TailwindCSS-4.x-38B2AC?logo=tailwind-css&logoColor=white)](https://tailwindcss.com)
[![SQLAlchemy](https://img.shields.io/badge/SQLAlchemy-2.0+-D71F00?logo=sqlalchemy&logoColor=white)](https://www.sqlalchemy.org)
[![PostgreSQL](https://img.shields.io/badge/PostgreSQL-16-4169E1?logo=postgresql&logoColor=white)](https://www.postgresql.org)
[![Docker](https://img.shields.io/badge/Docker-Enabled-2496ED?logo=docker&logoColor=white)](https://www.docker.com)

**HRFlow AI** is an enterprise-grade autonomous HR Workflow Automation Platform. It unifies talent acquisition, employee onboarding, performance management, skill gap diagnostics, and statutory compliance into a deterministic, event-driven automation engine paired with an authorized AI HR Copilot.

---

## 1. Product Overview

HRFlow AI is designed as a **mission-critical automation and orchestration engine**, not merely an administrative CRUD recordkeeper. It coordinates multi-step operational pipelines across departments with strict SLA timers, condition branching, escalation tiers, multi-channel notifications, and comprehensive auditability.

### Primary Operational Objectives
- **Zero-Touch Execution:** Routine HR processes execute deterministically based on event triggers (e.g., candidate shortlisted, employee hired, deadline approaching).
- **Human-in-the-Loop Governance:** High-impact and destructive actions (status changes, offers, contract terminations, external dispatches) require explicit confirmation and role-authorized approval.
- **Provider Agnostic:** Plug-and-play abstractions for Large Language Models (OpenAI, Anthropic, Gemini, Mock) and Email Services (SMTP, SendGrid, Mailgun, AWS SES).
- **Enterprise Security & Audit:** Complete RBAC (6 discrete roles), rate limiting, strict security headers, and an immutable statutory audit trail.

---

## 2. Features

### Core Modules
1. **Automation Hub:** Centralized command center providing real-time workflow telemetry, execution success rates, active states, step run histories, and live activity streams.
2. **Visual Workflow Canvas:** Interactive node-based drag-and-drop / sequential builder supporting triggers, conditional logic, multi-stage actions, delay timers, and reordering.
3. **Recruitment Automation:** Automated candidate resume parsing, algorithmic scoring against job specs, shortlisting workflows, and auto-scheduling interview invitations.
4. **Employee Lifecycle Orchestration:** Structured transitions across 9 distinct lifecycle stages (`Candidate` $\rightarrow$ `Hired` $\rightarrow$ `Onboarding` $\rightarrow$ `Active` $\rightarrow$ `Review` $\rightarrow$ `Development` $\rightarrow$ `Offboarding` $\rightarrow$ `Exited`).
5. **30-60-90 Day Onboarding Ramp:** Automated asset provisioning, IT credential checklist verification, orientation scheduling, buddy pairing, and multi-tier delinquency escalations.
6. **Performance & Goal Management:** Multi-phase review cycles (`Self-Review` $\rightarrow$ `Manager Review` $\rightarrow$ `HR Sign-off`), KPI tracking, scoring weights, and personal development plans.
7. **Academy & Skill Gap Diagnostics:** Role-based competency matrices, automated proficiency gap analysis, training recommendations, HR approval queues, and auto-skill record promotion upon course completion.
8. **Statutory Compliance Automation:** Automated 30d/15d warning reminders, 7d critical alerts, overdue breach escalation, evidence collection, and immutable compliance audit ledger.
9. **Authorized AI HR Copilot:** Conversational assistant grounded in authorized database entities that answers operational queries, drafts workflows, and proposes actions with two-step approval modals.
10. **Multi-Channel Notification Engine:** In-app inbox, variable-interpolated templates (`{{employee_name}}`, `{{deadline}}`), delivery status telemetry, exponential backoff retries, and provider fallback.
11. **Workforce Intelligence & Telemetry:** Dynamic conversion funnels, SLA breach metrics, departmental compliance heatmaps, and automation efficiency analytics computed directly from live database entities.
12. **Immutable Audit & Security Ledger:** Permanent, tamper-evident recording of logins, data mutations, workflow activations, AI recommendations, and compliance verifications.

---

## 3. Architecture

```text
                                  +-------------------------------------------------------+
                                  |                 Client Web Browser                    |
                                  |      React 19 / TypeScript / Tailwind CSS / Vite      |
                                  +---------------------------+---------------------------+
                                                              |
                                                    HTTPS / Reverse Proxy
                                                              |
                                  +---------------------------v---------------------------+
                                  |              Nginx Container (Port 80)                |
                                  |    - Static Asset Caching   - Gzip Compression        |
                                  |    - Security Headers       - SPA History Routing     |
                                  +---------------------------+---------------------------+
                                                              |
                                                      /api/v1 Reverse Proxy
                                                              |
                                  +---------------------------v---------------------------+
                                  |             FastAPI Backend (Port 8000)               |
                                  |                                                       |
                                  |  +-------------------------------------------------+  |
                                  |  |   Core Middleware: CORS, CSP, Rate Limiting     |  |
                                  |  +-------------------------------------------------+  |
                                  |  |   Security: JWT Validation, RBAC Authority      |  |
                                  |  +-------------------------------------------------+  |
                                  |  |   Services Layer:                               |  |
                                  |  |   - Workflow Engine      - AI Copilot Engine    |  |
                                  |  |   - Lifecycle Service    - Learning Diagnostic  |  |
                                  |  |   - Recruitment Engine   - Compliance Monitor   |  |
                                  |  |   - Email Provider Relay - Audit Service        |  |
                                  |  +-------------------------------------------------+  |
                                  +-------------+---------------------------+-------------+
                                                |                           |
                                      SQLAlchemy 2.0 ORM             Provider APIs
                                                |                           |
                    +---------------------------v-------------+   +---------v---------+
                    |        PostgreSQL Database (Port 5432)  |   | AI / LLM Services |
                    |      - 22 Normalized Relational Tables  |   | OpenAI / Anthropic|
                    |      - Alembic Schema Migrations        |   | Gemini / Mock LLM |
                    |      - Full Referential Integrity       |   +-------------------+
                    |      - Persistent Named Docker Volume   |   | Email Gateways    |
                    +-----------------------------------------+   | SendGrid / SES    |
                                                                  | SMTP Relay / Mock |
                                                                  +-------------------+
```

---

## 4. Technology Stack

### Backend
- **Runtime:** Python 3.11+
- **Web Framework:** FastAPI (Asynchronous ASGI)
- **ASGI Server:** Uvicorn (Worker Process Management)
- **ORM & Data Layer:** SQLAlchemy 2.0 (Declarative Mapping)
- **Schema Migrations:** Alembic
- **Validation & Serialization:** Pydantic v2 & Pydantic-Settings
- **Security & Cryptography:** Python-Jose (JWT HS256), Passlib (Bcrypt)
- **Database Adapters:** Psycopg2-Binary (PostgreSQL), SQLite3 (Local fallback)
- **Testing:** Pytest, AnyIO, HTTPX

### Frontend
- **Framework:** React 19 (Hooks, Context Architecture)
- **Language:** TypeScript 5.x
- **Build Tool:** Vite 8.x
- **Styling:** Tailwind CSS 4.x
- **Icons:** Lucide React
- **Data Visualizations:** Recharts
- **Workflow Graphs:** @xyflow/react
- **Forms & Validation:** React Hook Form, Zod

### DevOps & Infrastructure
- **Containerization:** Docker & Docker Compose
- **Web Server:** Nginx 1.27 Alpine (Multi-stage build)
- **Database Engine:** PostgreSQL 16 Alpine

---

## 5. Local Setup

### Prerequisites
- Python 3.11+
- Node.js 20+ and npm
- PostgreSQL 14+ (or zero-setup SQLite fallback)
- Git

### Clone Repository
```bash
git clone https://github.com/your-org/hrflow-ai.git
cd hrflow-ai
```

---

## 6. Environment Variables

Create a root `.env` file from the provided `.env.example`:

```bash
cp .env.example .env
```

### Key Environment Variables

| Variable | Required | Default / Example | Purpose |
| :--- | :---: | :--- | :--- |
| `DATABASE_URL` | **Yes** | `postgresql://hrflow_user:SecurePassword123!@localhost:5432/hrflow_db` | Relational database connection string |
| `JWT_SECRET` | **Yes** | `your-secure-random-jwt-secret-key-min-32-chars` | HMAC SHA-256 secret key for signing tokens |
| `AI_PROVIDER` | No | `mock` (`mock`, `openai`, `anthropic`, `gemini`) | LLM provider abstraction choice |
| `AI_API_KEY` | Conditional | `sk-your-ai-api-key-here` | API key required when using external LLM |
| `EMAIL_PROVIDER` | No | `mock` (`mock`, `smtp`, `sendgrid`, `mailgun`, `ses`) | Email delivery provider relay choice |
| `EMAIL_API_KEY` | Conditional | `your-email-provider-api-key` | API key for HTTP email dispatchers |
| `EMAIL_SEND_REAL`| No | `false` | Safety toggle: set `true` to dispatch live emails |
| `FRONTEND_URL` | No | `http://localhost:5173` | Allowed origin for CORS headers |
| `BACKEND_URL` | No | `http://127.0.0.1:8000` | Backend API base address |
| `VITE_API_BASE_URL` | No | `http://localhost:8000/api/v1` | Frontend build-time API prefix |

---

## 7. Database Setup

### Option A: PostgreSQL (Production Standard)
1. Ensure PostgreSQL is running and create the database user and database:
```sql
CREATE USER hrflow_user WITH PASSWORD 'SecurePassword123!';
CREATE DATABASE hrflow_db OWNER hrflow_user;
GRANT ALL PRIVILEGES ON DATABASE hrflow_db TO hrflow_user;
```

2. Set the `DATABASE_URL` in `.env`:
```env
DATABASE_URL=postgresql://hrflow_user:SecurePassword123!@localhost:5432/hrflow_db
```

3. Run Alembic migrations:
```bash
cd backend
python -m alembic upgrade head
cd ..
```

### Option B: SQLite (Instant Zero-Config Development)
If `DATABASE_URL` is omitted or points to `sqlite:///./hrflow.db`, the database engine automatically creates the SQLite file with full foreign-key enforcement and thread safety enabled.

### Seeding Data
Seed the base roles, departments, demo employees, workflows, and executions:
```bash
# Seed standard enterprise baseline:
python -m backend.app.seed

# Seed complete realistic demo dataset (35 employees, 28 candidates, 6 active workflows):
$env:PYTHONPATH = "backend"
python -m app.seed_demo
```

---

## 8. Running Backend

```bash
# Create virtual environment
python -m venv .venv

# Activate virtual environment
# Windows:
.venv\Scripts\activate
# Linux/macOS:
source .venv/bin/activate

# Install dependencies
pip install -r backend/requirements.txt

# Run backend development server
$env:PYTHONPATH = "backend"
python -m uvicorn app.main:app --host 127.0.0.1 --port 8000 --reload
```

The API documentation will be available at:
- **Swagger UI:** `http://127.0.0.1:8000/docs`
- **ReDoc:** `http://127.0.0.1:8000/redoc`
- **OpenAPI Schema:** `http://127.0.0.1:8000/openapi.json`

---

## 9. Running Frontend

```bash
cd frontend

# Install dependencies
npm install

# Start Vite development server
npm run dev
```

Open your browser to `http://localhost:5173`.

### Building Frontend for Production
```bash
npm run build
```
The optimized bundle will be compiled to `frontend/dist`.

---

## 10. Demo Credentials

The platform includes 6 seeded enterprise accounts covering the complete Role-Based Access Control (RBAC) hierarchy. All default demo accounts share the password: **`password123`**.

| Role | Email | Name | Capabilities & Scope |
| :--- | :--- | :--- | :--- |
| **SUPER_ADMIN** | `superadmin@hrflow.ai` | Alexandra Vance | Full platform control, security policies, user admin, audit logs |
| **HR_ADMIN** | `hradmin@hrflow.ai` | Marcus Sterling | Workflows, compliance certification, employee records, onboarding |
| **HRBP** | `hrbp@hrflow.ai` | Elena Rostova | Business partner oversight, performance cycles, skill gap approvals |
| **RECRUITER** | `recruiter@hrflow.ai` | Jordan Chen | Job requisitions, resume parsing, candidate scoring, interview scheduling |
| **HIRING_MANAGER**| `hiringmanager@hrflow.ai` | David Kim | Candidate reviews, interview feedback, team approvals, direct reports |
| **EMPLOYEE** | `employee@hrflow.ai` | Sophia Martinez | Self-service portal, self-review submissions, assigned training paths |

---

## 11. Deployment Instructions

### Method 1: Docker Compose (Recommended Production Stack)

Deploy the full stack (PostgreSQL + FastAPI backend + Nginx frontend) with a single command:

```bash
# 1. Review environment configuration
cp .env.example .env

# 2. Build and launch all containers in detached mode
docker-compose up --build -d

# 3. Verify container health
docker-compose ps

# 4. Seed demo dataset inside the backend container
docker-compose exec backend python -m app.seed_demo
```

Access the services:
- **Web Application:** `http://localhost` (Port 80)
- **Backend API:** `http://localhost:8000`
- **API Documentation:** `http://localhost:8000/docs`
- **PostgreSQL Database:** `localhost:5432`

To shut down the stack:
```bash
docker-compose down
# To also delete persistent database volume:
docker-compose down -v
```

### Method 2: Standalone Production Container Deployment

#### Backend Container
```bash
docker build -t hrflow-backend:latest ./backend
docker run -d \
  --name hrflow-backend \
  -p 8000:8000 \
  -e DATABASE_URL="postgresql://user:pass@db-host:5432/hrflow_db" \
  -e JWT_SECRET="your-32-character-secure-random-jwt-secret" \
  -e ENVIRONMENT="production" \
  hrflow-backend:latest
```

#### Frontend Container
```bash
docker build -t hrflow-frontend:latest ./frontend
docker run -d \
  --name hrflow-frontend \
  -p 80:80 \
  hrflow-frontend:latest
```

---

## 12. Workflow Examples

HRFlow AI executes deterministic, multi-branching automated workflows. Below are the 5 mandatory automated workflows operational in the platform:

### 1. Candidate Screening Automation
```mermaid
flowchart TD
    A[Trigger: Candidate Shortlisted] --> B{Condition: Match Score >= 80%?}
    B -- Yes --> C[Action: Generate Interview Invitation]
    C --> D[Action: Notify Hiring Manager via Email]
    D --> E[Action: Create Feedback Task with 2-Day SLA]
    E --> F{Delay: 48h Check Feedback}
    F -- Feedback Missing --> G[Action: Dispatch Reminder]
    G --> H[Action: Escalate Delinquency to Lead Recruiter]
    B -- No --> I[Action: Stage in Talent Pool]
```

### 2. Employee Onboarding & 30-60-90 Day Milestone Ramp
```mermaid
flowchart TD
    A[Trigger: Candidate Moved to Hired] --> B[Action: Generate Employee Dossier & Code]
    B --> C[Action: Issue Document Collection Tasks]
    C --> D[Action: Provision IT Credentials & Equipment Checklist]
    D --> E[Action: Schedule Day-1 Orientation & Assign Buddy]
    E --> F[Action: Schedule Day-30 Ramp Milestone Review]
    F --> G[Action: Schedule Day-60 Competency Evaluation]
    G --> H[Action: Schedule Day-90 Full Integration Signoff]
```

### 3. Skill Gap & Adaptive Training Automation
```mermaid
flowchart TD
    A[Trigger: Skill Gap Identified via Diagnostic] --> B[Action: Generate Training Recommendation]
    B --> C[Approval Gate: Await HRBP Sign-off]
    C -- Approved --> D[Action: Enroll in Academy Curriculum]
    D --> E[Action: Set Due Date & Monitor SLA]
    E --> F[Action: Employee Completes Coursework]
    F --> G[Action: Auto-Update Skill Proficiency Record in Database]
```

### 4. Statutory Compliance & Deadline Escalation
```mermaid
flowchart LR
    A[Deadline Window] --> B[T-30 Days: Automated Reminder]
    B --> C[T-15 Days: Urgent Warning]
    C --> D[T-7 Days: Critical Escalation to Dept Head]
    D --> E[Deadline Reached: Breach Alert & Audit Log]
    E --> F[Evidence Verified: Certified Compliant & Closed]
```

### 5. Multi-Phase Performance Review Governance
```mermaid
flowchart TD
    A[Trigger: Review Cycle Initiated] --> B[Action: Notify Employee to Submit Self-Review]
    B --> C[Action: Employee Submits Self-Score & Comments]
    C --> D[Action: Notify Manager for Evaluation]
    D --> E[Action: Manager Submits Rating & Dev Plan]
    E --> F[Action: Forward to HR for Final Sign-off]
    F --> G[Action: Composite Score & Development Plan Finalized]
```

---

## 13. Known Limitations

1. **Simulated Sandbox Email Delivery by Default:** To prevent inadvertent external transmissions during development, `EMAIL_SEND_REAL` defaults to `false`. Outbound dispatches are recorded in the notification history and delivery logs table with `SIMULATED` status until `EMAIL_SEND_REAL=true` and active credentials are supplied.
2. **Synchronous Delay Evaluation:** In the local environment, delay timers (e.g., "Wait 2 days") are evaluated via periodic sweeps (`/automation-sweep`, `/check-overdue`) or synchronous workflow runs. In a scaled multi-region deployment, backing queue orchestrators (e.g., Celery, Temporal, or AWS SQS) should be integrated.
3. **Local File Storage Driver:** Uploaded employee documents, compliance certifications, and candidate CVs are stored on the local filesystem (`/documents`, `/uploads`) with SHA-256 integrity verification. For cloud-native deployments, configure an S3-compatible object storage provider.
4. **LLM Provider Quota Limits:** When switching `AI_PROVIDER` from `mock` to `openai` or `anthropic`, ensure appropriate rate limits and spending limits are configured on your external provider account to prevent throttling during heavy copilot usage.
5. **Single-Organization Scope:** The current schema operates as single-tenant per deployment instance. Multi-tenant partitioning (`tenant_id` column on all tables with row-level security) can be layered on for multi-tenant SaaS offerings.

---

## 📄 License & Attribution

HRFlow AI is licensed under the Apache License 2.0. Built with FastAPI, React 19, SQLAlchemy, Tailwind CSS, and Lucide Icons.
