# Contributing to HRFlow AI

Thank you for your interest in contributing to **HRFlow AI**! This document provides standards and workflows for submitting bug fixes, new features, and improvements.

---

## 1. Development Principles

1. **Automation First:** HRFlow AI is an automation engine. Features should be designed around event triggers, conditional evaluation, and deterministic execution rather than simple CRUD.
2. **Strict RBAC Integrity:** Never bypass role-based authorization. Ensure endpoints enforce permissions across the 6 system roles.
3. **No Regressions:** All existing Pytest test suites (111 tests) and frontend builds must pass before any pull request is merged.
4. **Clean Code & Typings:** Use strict TypeScript types (no `any` where avoidable) and Pydantic v2 schemas for all API payloads.

---

## 2. Setting Up Your Local Environment

### Fork and Clone
```bash
git clone https://github.com/your-username/hrflow-ai.git
cd hrflow-ai
git checkout -b feature/your-feature-name
```

### Backend Setup
```bash
python -m venv .venv
source .venv/bin/activate  # Or .venv\Scripts\activate on Windows
pip install -r backend/requirements.txt

# Run migrations
cd backend && python -m alembic upgrade head && cd ..

# Seed demo dataset
export PYTHONPATH=backend
python -m app.seed_demo

# Run tests
pytest backend/tests -v
```

### Frontend Setup
```bash
cd frontend
npm install
npm run build
npm run dev
```

---

## 3. Pull Request Guidelines

1. **Branch Naming:**
   - Features: `feature/short-description`
   - Bug fixes: `fix/short-description`
   - Refactor: `refactor/short-description`
   - Documentation: `docs/short-description`

2. **Commit Messages:** Follow [Conventional Commits](https://www.conventionalcommits.org/):
   - `feat: add candidate scoring threshold trigger`
   - `fix: resolve review cycle date calculation off-by-one`
   - `docs: update deployment environment runbook`
   - `test: add unit tests for compliance sweep`

3. **Pre-Submission Checklist:**
   - [ ] Backend tests pass: `pytest backend/tests`
   - [ ] Frontend builds with 0 errors: `npm run build --prefix frontend`
   - [ ] No secrets or `.env` files committed
   - [ ] Alembic migration added if models were modified
   - [ ] Documentation updated if APIs or configuration changed

---

## 4. Code of Conduct

We are committed to providing a welcoming, inclusive, and harassment-free environment for all contributors. Please be respectful and collaborative in all discussions and code reviews.
