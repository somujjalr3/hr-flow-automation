# HRFlow AI — REST API Specification & Reference

**Base URL:** `/api/v1`  
**Authentication Scheme:** Bearer JWT Token (`Authorization: Bearer <TOKEN>`)  
**Data Format:** `application/json`  

---

## 1. Authentication & Security

### `POST /auth/login`
Authenticates a user and returns a signed JWT access token.

- **Request Body:**
  ```json
  {
    "email": "superadmin@hrflow.ai",
    "password": "password123"
  }
  ```
- **Response (200 OK):**
  ```json
  {
    "access_token": "eyJhbGciOiJIUzI1NiIsIn...",
    "token_type": "bearer",
    "user": {
      "id": 1,
      "email": "superadmin@hrflow.ai",
      "name": "Alexandra Vance",
      "role": "SUPER_ADMIN",
      "department_id": 1
    }
  }
  ```

### `GET /auth/me`
Retrieves profile and permissions for the currently authenticated identity.
- **Headers:** `Authorization: Bearer <token>`
- **Response (200 OK):** User object.

---

## 2. Workflows & Automation Engine

### `GET /workflows`
Lists all configured workflow definitions with versioning, category, and active status.
- **Query Parameters:**
  - `category` (Optional): `Recruitment`, `Onboarding`, `Performance`, `Training`, `Compliance`, `Employee Lifecycle`
  - `status` (Optional): `Active`, `Draft`, `Inactive`
- **Response (200 OK):** Array of workflow objects.

### `POST /workflows`
Creates a new workflow definition.
- **Allowed Roles:** `SUPER_ADMIN`, `HR_ADMIN`, `HRBP`, `RECRUITER`
- **Request Body:**
  ```json
  {
    "title": "Senior Engineer Screening Pipeline",
    "description": "Shortlist trigger with automated delay and escalation",
    "category": "Recruitment",
    "trigger_type": "CANDIDATE_SHORTLISTED",
    "steps": [
      {
        "step_order": 1,
        "step_type": "ACTION",
        "action_type": "SEND_INVITATION",
        "config": { "template": "interview_invitation" }
      },
      {
        "step_order": 2,
        "step_type": "DELAY",
        "config": { "duration_days": 2 }
      },
      {
        "step_order": 3,
        "step_type": "ESCALATION",
        "action_type": "ESCALATE_DELINQUENCY",
        "config": { "escalate_to": "HIRING_MANAGER" }
      }
    ]
  }
  ```

### `POST /workflows/{id}/execute`
Synchronously triggers or simulates execution of a workflow against real or mock entity payload.
- **Request Body:**
  ```json
  {
    "candidate": {
      "id": 101,
      "name": "Jordan Miller",
      "score": 85,
      "email": "jordan.miller@example.com"
    },
    "feedback": { "status": "completed" }
  }
  ```
- **Response (200 OK):**
  ```json
  {
    "id": 1578,
    "workflow_id": 1,
    "status": "COMPLETED",
    "started_at": "2026-10-25T06:17:48.123456",
    "completed_at": "2026-10-25T06:17:48.234567",
    "step_results": [
      {
        "step_id": 1,
        "status": "SUCCESS",
        "action": "SEND_INVITATION",
        "details": "Invitation sent to jordan.miller@example.com"
      }
    ]
  }
  ```

### `POST /workflows/{id}/toggle-active`
Toggles active execution status of a workflow definition.

---

## 3. Recruitment & Screening

- `GET /recruitment/candidates`: Filter candidates by job requisition, status, and minimum score threshold.
- `POST /recruitment/candidates`: Create a candidate profile.
- `POST /recruitment/candidates/{id}/parse-cv`: Trigger automated keyword and score extraction against job description.
- `POST /recruitment/candidates/{id}/shortlist`: Advance candidate to Shortlisted status (triggers candidate screening automation).
- `POST /recruitment/jobs`: Create open job position requisitions.

---

## 4. Employee Directory & Lifecycle

- `GET /employees`: Retrieve organizational directory.
- `GET /employees/{id}`: Detailed employee dossier including skills, OKRs, documents, and manager.
- `POST /employees/{id}/lifecycle-stage`: Advance lifecycle stage (e.g. `Hired` $\rightarrow$ `Onboarding`).
- `POST /employees/{id}/automate-onboarding`: Automatically generates all 8 onboarding components (dossier, IT tasks, orientation, milestone reviews).
- `POST /employees/{id}/offboard`: Initiate structured asset recovery and exit interview pipeline.

---

## 5. Performance & Learning

- `GET /performance/reviews`: Retrieve review evaluations across departments.
- `POST /performance/cycles`: Launch quarterly/annual evaluation cycles.
- `POST /performance/reviews/{id}/self-review`: Submit self-evaluation score and comments.
- `POST /performance/reviews/{id}/manager-review`: Submit manager evaluation, rating, and development plan.
- `POST /performance/reviews/{id}/signoff`: Submit final HR review signoff.
- `GET /training/skill-gap/{employee_id}`: Run proficiency diagnostic against required role benchmarks.
- `POST /training/skill-gap/{employee_id}/recommend`: Generate training course recommendations.
- `POST /training/recommendations/{id}/approve`: HR approval that enrolls employee in academy pathway.
- `POST /training/assignments/{id}/complete`: Mark course completed and update employee skill record in database.

---

## 6. Statutory Compliance

- `GET /compliance/items`: Retrieve compliance requirements and status.
- `POST /compliance/items`: Create statutory compliance requirement item.
- `POST /compliance/items/{id}/complete`: Submit evidence URL/notes and certify compliant.
- `GET /compliance/items/{id}/audit-trail`: Retrieve immutable audit history for an individual item.
- `POST /compliance/automation-sweep`: Run 30d/15d/7d reminder and breach sweep.

---

## 7. AI HR Copilot

### `POST /copilot/query`
Sends an operational query to the copilot.
- **Request Body:**
  ```json
  {
    "query": "Show overdue compliance tasks",
    "thread_id": "optional-thread-uuid"
  }
  ```
- **Response (200 OK):**
  ```json
  {
    "answer": "There are currently 2 statutory compliance tasks past deadline...",
    "data": [ ... ],
    "proposed_action": {
      "action_id": "act_8271",
      "action_type": "SEND_REMINDER",
      "title": "Dispatch Overdue Notice",
      "requires_confirmation": true
    }
  }
  ```

### `POST /copilot/actions/{id}/confirm`
Executes a confirmed high-impact action proposed by the Copilot.

---

## 8. Multi-Channel Notifications

- `GET /notifications`: Lists in-app notifications for authenticated user.
- `POST /notifications/{id}/read`: Mark notification as read.
- `POST /notifications/dispatch-from-template`: Dispatch notification using variable interpolation.
- `GET /notifications/provider-status`: Retrieve email provider abstraction status (`MOCK`, `SMTP`, `SENDGRID`, `MAILGUN`, `SES`).

---

## 9. Security Audit Logs

- `GET /audit-logs`: Query immutable audit records with filters (`user_id`, `action`, `entity`, `module`, `date_range`, `search`).
- `GET /audit-logs/stats`: High-level security statistics (logins, mutations, AI actions).
