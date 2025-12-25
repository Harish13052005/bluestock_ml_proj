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

git clone <your-repo-url> bluestock-ml-project
cd bluestock-ml-project/backend
python -m venv venv

Windows
venv\Scripts\activate

Linux/Mac
source venv/bin/activate
pip install -r requirements.txt


Configure PostgreSQL:

CREATE DATABASE bluestock_db;
CREATE ROLE bluestock_user WITH LOGIN PASSWORD 'your_password';
GRANT ALL PRIVILEGES ON DATABASE bluestock_db TO bluestock_user;


Update `bluestock_config/settings.py`:


Update `bluestock_config/settings.py`:

DATABASES = {
"default": {
"ENGINE": "django.db.backends.postgresql",
"NAME": "bluestock_db",
"USER": "bluestock_user",
"PASSWORD": "your_password",
"HOST": "127.0.0.1",
"PORT": "5432",
}
}


Create `.env` in `backend/`:

API_KEY=your_bluestock_api_key
Run migrations and start the server:

python manage.py migrate
python manage.py runserver


Backend is available at `http://127.0.0.1:8000/`.

---

## 4. Frontend Setup

cd ../frontend
npm install
npm start


Frontend is available at `http://localhost:3000/`.

---

## 5. API Endpoints

### 5.1 Analyze Companies

- **URL:** `POST /api/analyze/?format=json`  
- **Body:**

{
"company_ids": ["TCS", "HDFCBANK", "INFY"]
}


- **Response (simplified):**

{
"status": "success",
"count": 3,
"results": [
{
"company_id": "TCS",
"company_name": "Tata Consultancy Services Ltd",
"health_rating": "NEUTRAL",
"pros": ["Company is almost debt free.", "..."],
"cons": ["Stock is trading at 15.2 times its book value", "..."],
"metrics_summary": {
"sales_growth_10y": "10 Years: 11%",
"roe_10y": "10 Years: 40%"
},
"details_url": "https://bluemutualfund.in/app1/pages/company.php?id=TCS"
}
]
}


### 5.2 Get Stored Analyses

- **URL:** `GET /api/results/?format=json`  
- **Description:** Returns all companies stored in the `Analysis` table with their latest health rating and pros/cons.

---

## 6. Machine Learning Analysis Module

1. Backend fetches Bluestock data for each `company_id`.  
2. Data is cleaned (uppercase IDs, numeric conversion, NULL removal).  
3. A rule‑based analyzer evaluates ROE, growth, debt and dividend indicators to:
   - Generate additional ML‑style pros and cons.  
   - Assign a **GOOD / NEUTRAL / BAD** health rating.  
4. Bluestock pros/cons from the API are merged with ML pros/cons and stored in PostgreSQL.  
5. Combined insights are returned to the React frontend.

---

## 7. Frontend Features

- Input box to add company symbols (e.g., `TCS`, `HDFCBANK`, `INFY`) and chip UI to add/remove them.  
- **Analyze** button to call the backend and display results.  
- Card per company showing:
  - Company name and health badge (GOOD / NEUTRAL / BAD).  
  - Bluestock metrics such as **10Y Sales** and **10Y ROE**.  
  - Detailed **Pros** and **Cons** lists.  
  - “View full Bluestock analysis →” link to the official Bluestock page.  
- Filter bar (`ALL / GOOD / NEUTRAL / BAD`) to view companies by rating.

---

## 8. Testing

### API (Postman)

- `POST /api/analyze/` – test valid and invalid payloads.  
- `GET /api/results/` – verify persistence and response shape.

### UI

- Try different combinations of symbols (single, multiple, invalid, duplicates).  
- Confirm cards, filters, loading state and error messages behave correctly.

---

## 9. Environment Template

Example `.env.example` for the backend:

API_KEY=your_bluestock_api_key
DB_NAME=bluestock_db
DB_USER=bluestock_user
DB_PASSWORD=your_password
DB_HOST=127.0.0.1
DB_PORT=5432
