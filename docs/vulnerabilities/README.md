# DevSecOps Pipeline — OWASP Juice Shop

**Module:** IE3142 — DevOps Security  
**Assignment:** Building and Securing a DevSecOps Pipeline  
**Team:** [Member 1] [Member 2] [Member 3] [Member 4]  
**Submission date:** 1st October 2026

---

## 📋 Project Overview

This repository contains a secured fork of **OWASP Juice Shop** with four demonstrated vulnerabilities exploited, fixed, and verified through a GitHub Actions CI/CD pipeline. The project demonstrates the DevSecOps mindset in practice: each vulnerability is proven working before the fix, then proven neutralised afterwards, with SAST evidence captured before and after each change.

The pipeline automates four security gates on every push:

- **SAST** — Semgrep
- **Dependency scanning** — `npm audit`
- **Secrets scanning** — Gitleaks
- **Container image scanning** — Trivy

---

## 🎯 Vulnerabilities Demonstrated & Fixed

| # | Vulnerability | CWE | OWASP | File | Status |
|---|---|---|---|---|---|
| 1 | SQL Injection in Login (Authentication Bypass) | CWE-89 | A03:2021 | `routes/login.ts` | ✅ Fixed |
| 2 | Union-Based SQL Injection in Product Search | CWE-89 | A03:2021 | `routes/search.ts` | ✅ Fixed |
| 3 | DOM-Based XSS in Search | CWE-79 | A03:2021 | `frontend/src/app/search-result/search-result.component.ts` | ✅ Fixed |
| 4 | Forged Review (Broken Access Control / Mass Assignment) | CWE-284 / CWE-915 | A01:2021 | `routes/createProductReviews.ts` | ✅ Fixed |

Each vulnerability is documented with before/after screenshots, Semgrep finding counts, and the exact code change that neutralised it.

---

## 🏗️ System Architecture

OWASP Juice Shop is a deliberately vulnerable Node.js / Angular web application. Two communicating components:

- **Frontend** — Angular SPA (served from `frontend/dist/frontend`)
- **Backend** — Express API + SQLite database (default) or MongoDB (reviews)

Deployment: single Docker container built from the repo's existing `Dockerfile`.

---

## 🐳 Running the Application

### Prerequisites

- Docker (with `docker compose` plugin)
- Node.js 24+ (for local development, optional)

Install Docker on Kali/Debian:

```bash
sudo apt update
sudo apt install -y docker.io docker-compose-plugin
sudo systemctl enable docker --now
sudo usermod -aG docker $USER
newgrp docker
