# CostGuard 🛡️
> **"Know the Cost Before You Deploy."**
> Cloud Infrastructure Cost Intelligence & Financial Circuit Breaker for Terraform

[![FastAPI](https://img.shields.io/badge/Backend-FastAPI-009688?style=flat-square&logo=fastapi)](https://fastapi.tiangolo.com)
[![React](https://img.shields.io/badge/Frontend-React%20%2B%20Vite-61DAFB?style=flat-square&logo=react)](https://react.dev)
[![Tailwind CSS](https://img.shields.io/badge/Styling-Tailwind%20CSS-38B2AC?style=flat-square&logo=tailwind-css)](https://tailwindcss.com)
[![SQLite](https://img.shields.io/badge/Cache-SQLite-003B57?style=flat-square&logo=sqlite)](https://sqlite.org)
[![Azure Retail Prices API](https://img.shields.io/badge/Pricing-Azure%20Retail%20API-0078D4?style=flat-square&logo=microsoft-azure)](https://prices.azure.com/api/retail/prices)

---

## ⚡ What is CostGuard?

CostGuard is a FinOps shift-left automation platform that analyzes Terraform plans *before* infrastructure is provisioned. It intercepts cost spikes by fetching real-time retail pricing from Azure, checking against a local SQLite cache, calculating projected 730-hour monthly cloud expenditures, and enforcing automated **Financial Circuit Breakers** to block deployments that breach budget guardrails.

---

## 🚀 Key Features

- **Terraform Plan Parser**: Extracts resource actions (`CREATE`, `UPDATE`, `DELETE`, `REPLACE`), SKU types, and Azure locations from Terraform JSON plan output (`resource_changes[]`).
- **Official Azure Retail Prices API**: Fetches consumption hourly rates directly from `https://prices.azure.com/api/retail/prices` without requiring authentication.
- **High-Speed SQLite Pricing Cache**: Caches SKU prices locally to deliver sub-millisecond response times and offline resilience.
- **730-Hour Standard Cost Calculator**: Accurately computes monthly run rates across complex compute and storage resources.
- **Financial Circuit Breaker Guardrail**: Compares net monthly increase against a policy threshold (e.g., $50.00/mo). If the budget is breached, the deployment is marked `FAILED` and locked with `DEPLOYMENT BLOCKED`.
- **Interactive Dark SaaS Dashboard**: Built with React, Vite, Tailwind CSS, and Lucide icons featuring live health pulses, status badges, breakdown tables, architecture maps, and SQLite cache viewers.
- **Hackathon Demo Controls**: Single-click buttons to run standard demo analysis, simulate budget breaches, and reset states.

---

## 🏗️ Architecture Pipeline

```text
Terraform Plan (JSON)
       ↓
Resource Extractor (SKU & Region Normalization)
       ↓
SQLite Cache (costguard.db)  ←→  Azure Retail Prices API (Live)
       ↓
Monthly Cost Calculator (730 hrs/month)
       ↓
Budget Guardrail (Circuit Breaker Evaluation)
       ↓
Status: PASSED (Deployment Allowed) / FAILED (Deployment Blocked)
```

---

## 🛠️ Tech Stack

- **Frontend**: React 18, Vite 5, Tailwind CSS 3, Lucide React
- **Backend**: Python 3.11+, FastAPI, Uvicorn, SQLite, requests/httpx, Pydantic
- **Data Source**: Azure Retail Prices API (`https://prices.azure.com/api/retail/prices`)

---

## 🏁 Quickstart Guide

### 1. Start the FastAPI Backend

```bash
cd costguard/backend
pip install -r requirements.txt
uvicorn main:app --reload --port 8000
```
Backend will be available at: `http://localhost:8000`  
Swagger API Docs: `http://localhost:8000/docs`

### 2. Start the React Frontend

```bash
cd costguard/frontend
npm install
npm run dev
```
Frontend will be available at: `http://localhost:5173`

---

## 📡 API Endpoints

| Method | Endpoint | Description |
|---|---|---|
| `GET` | `/` | API status and service information |
| `GET` | `/health` | Healthcheck (Azure API status, DB cache status) |
| `POST` | `/analyze` | Ingests Terraform plan JSON, calculates monthly delta & guardrails |
| `GET` | `/cache` | Retrieves all cached SKUs and hit statistics |
| `POST` | `/demo` | Runs instant demo evaluation (`?mode=passed` or `?mode=breach`) |
| `GET` | `/demo/plan` | Returns raw demo plan JSON payloads |

---

## 🧪 Demo Scenario Data

```text
Current Monthly Cost:      $15.18
Projected Monthly Cost:    $50.08
Net Monthly Delta:        +$34.90
Budget Limit:              $50.00
Guardrail Decision:        PASSED (34.90 <= 50.00) -> Deployment Allowed

Breach Scenario:
Projected Monthly Cost:    $90.18
Net Monthly Delta:        +$75.00
Budget Limit:              $50.00
Guardrail Decision:        FAILED (75.00 > 50.00) -> DEPLOYMENT BLOCKED
```

---

## 📜 License
MIT License. Built for Hackathon 2026.
