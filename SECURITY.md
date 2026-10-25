# Security Policy

The HRFlow AI team takes the security and integrity of our workflow automation platform and employee data seriously.

---

## 1. Supported Versions

We provide security updates and patches for the following versions:

| Version | Supported          |
| ------- | ------------------ |
| 1.0.x   | :white_check_mark: |
| < 1.0   | :x:                |

---

## 2. Reporting a Vulnerability

**Please do not report security vulnerabilities through public GitHub issues.**

If you discover a vulnerability or security-relevant issue in HRFlow AI:

1. **Email:** Send details to `security@hrflow.ai` (or open a private security advisory on GitHub).
2. **Details to Include:**
   - Detailed description of the vulnerability.
   - Proof of concept or reproduction steps.
   - Potential impact on confidential employee or candidate records.
   - Suggested mitigations if known.
3. **Response Timeline:**
   - Initial acknowledgement: Within **24 hours**.
   - Assessment and triage: Within **72 hours**.
   - Patch release and disclosure plan: Coordinated with the reporter.

---

## 3. Security Architecture Highlights

- **Authentication:** HS256 JWT tokens with configurable expiration, bcrypt password hashing, and token invalidation.
- **Access Control:** Multi-tier RBAC enforced on every API route via FastAPI dependencies.
- **Input Validation:** Strict Pydantic v2 schemas and validation for all API inputs and file uploads.
- **Security Headers:** Enforced via middleware (`X-Frame-Options: DENY`, `X-Content-Type-Options: nosniff`, `Strict-Transport-Security`, `Content-Security-Policy`).
- **Audit Trails:** Immutable logging of all state mutations, authentications, and compliance verifications.
