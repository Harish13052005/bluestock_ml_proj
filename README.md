# Bluestock ML Financial Analysis Dashboard

A full‑stack fintech project that fetches real stock fundamentals from the Bluestock API, applies rule‑based ML‑style analysis, stores results in a database, and displays insights on a React dashboard.

---

## 1. Architecture Overview

- **Backend:** Django + Django REST Framework  
  - Connects to Bluestock company API `company.php?id={company_id}&api_key=...` to fetch fundamentals and official pros/cons.  
  - Cleans and aggregates the data and runs a rule‑based analysis module.  
  - Classifies each company as **GOOD / NEUTRAL / BAD** and stores results in PostgreSQL.  
  - Exposes REST endpoints for analysis and history retrieval.

- **Database:** PostgreSQL  
  - `Company` table – basic information and selected financial metrics.  
  - `Analysis` table – health rating, combined pros/cons, and timestamp.

- **Frontend:** React + Axios  
  - Single‑page dashboard for entering Nifty100 company symbols.  
  - Calls backend APIs and renders cards with Bluestock metrics plus ML insights.

---

## 2. Tech Stack

- Python 3.11  
- Django, Django REST Framework  
- PostgreSQL  
- requests, python‑dotenv  
- React (create‑react‑app), Axios, CSS

---

## 3. Backend Setup

