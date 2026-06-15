# CustomerIQ – Customer Purchase Prediction & Campaign Analytics

A full-stack web application for predicting customer purchase behavior, segmenting customers using RFM analysis, and tracking marketing campaign effectiveness.

![Python](https://img.shields.io/badge/Python-3.11+-blue?logo=python)
![Flask](https://img.shields.io/badge/Flask-3.0-black?logo=flask)
![MySQL](https://img.shields.io/badge/MySQL-8.0-orange?logo=mysql)
![scikit-learn](https://img.shields.io/badge/scikit--learn-1.5-F7931E?logo=scikitlearn)
![Bootstrap](https://img.shields.io/badge/Bootstrap-5.3-7952B3?logo=bootstrap)

---

## Features

| Module | Highlights |
|---|---|
| **Dashboard** | Live KPI cards – revenue, customers, conversion rate; Chart.js charts |
| **Customer 360** | Segmented list view, per-customer purchase history & lifetime value |
| **Purchase Prediction** | Gradient Boosting model scoring each customer's buy probability |
| **RFM Segmentation** | Automated Recency-Frequency-Monetary scoring + KMeans clustering |
| **Campaign Analytics** | Conversion rates, ROI, daily response funnel per campaign |
| **REST API** | JSON endpoints for every chart & prediction (Power BI ready) |

---

## Tech Stack

- **Backend**: Python 3.11, Flask 3, Flask-SQLAlchemy, Flask-Migrate
- **Database**: MySQL 8 (SQLite for tests)
- **ML / Data**: scikit-learn (Gradient Boosting), Pandas, NumPy, Matplotlib
- **Frontend**: Bootstrap 5.3, Chart.js 4, Bootstrap Icons
- **BI**: REST API endpoints compatible with Power BI DirectQuery

---

## Project Structure

```
customer-purchase-prediction/
├── app/
│   ├── __init__.py          # App factory
│   ├── models.py            # SQLAlchemy ORM models
│   ├── routes/
│   │   ├── main.py          # Dashboard
│   │   ├── customers.py     # Customer CRUD + API
│   │   ├── campaigns.py     # Campaign performance
│   │   ├── predictions.py   # ML scoring endpoints
│   │   └── analytics.py     # Chart data APIs
│   ├── templates/           # Jinja2 HTML templates
│   └── static/              # CSS + JS
├── ml_models/
│   ├── predictor.py         # GradientBoosting pipeline
│   ├── segmentation.py      # RFM + KMeans segmentor
│   └── saved_models/        # Serialised .pkl files (git-ignored)
├── sql/
│   ├── schema.sql           # MySQL DDL
│   └── queries.sql          # Analytical SQL queries
├── scripts/
│   ├── seed_data.py         # Generates synthetic data
│   └── train_model.py       # Trains & evaluates the ML model
├── tests/
│   └── test_routes.py       # Pytest test suite
├── notebooks/               # Jupyter EDA notebooks (add your own)
├── reports/                 # Auto-generated Matplotlib charts
├── config.py
├── run.py
└── requirements.txt
```

---

## Quick Start

### 1. Clone & install

```bash
git clone https://github.com/YOUR_USERNAME/customer-purchase-prediction.git
cd customer-purchase-prediction
python -m venv venv
source venv/bin/activate        # Windows: venv\Scripts\activate
pip install -r requirements.txt
```

### 2. Configure environment

```bash
cp .env.example .env
# Edit .env with your MySQL credentials
```

### 3. Set up the database

```bash
# Option A – Flask-Migrate (recommended)
flask db init
flask db migrate -m "initial"
flask db upgrade

# Option B – run schema.sql directly
mysql -u root -p < sql/schema.sql
```

### 4. Seed with synthetic data

```bash
python scripts/seed_data.py
# Creates 500 customers, ~4,000 purchases, 15 campaigns
```

### 5. Train the ML model

```bash
python scripts/train_model.py
# Trains Gradient Boosting, saves pkl, generates RFM charts in /reports
```

### 6. Run the app

```bash
python run.py
# Open http://localhost:5000
```

---

## Power BI Integration

All chart endpoints return JSON and are compatible with Power BI's **Web connector** or a **DirectQuery** REST source.

| Endpoint | Description |
|---|---|
| `GET /analytics/api/revenue-trend` | Monthly revenue & transactions |
| `GET /analytics/api/channel-breakdown` | Sales by channel |
| `GET /analytics/api/category-performance` | Top product categories |
| `GET /analytics/api/segment-revenue` | Revenue per customer segment |
| `GET /campaigns/api/performance` | Campaign KPIs |
| `GET /campaigns/api/roi` | Budget vs revenue per campaign |
| `GET /customers/api/segments` | Segment distribution |
| `POST /predictions/batch` | Top 50 purchase candidates |

---

## ML Model Details

The purchase prediction model is a **scikit-learn Pipeline** containing:

1. `StandardScaler` – normalises numerical features
2. `GradientBoostingClassifier` – 200 estimators, learning rate 0.05, max depth 4

**Features used (8 total)**

| Feature | Description |
|---|---|
| `age` | Customer age |
| `income` | Annual income |
| `gender_encoded` | Binary encoding |
| `total_spent` | Lifetime spend |
| `purchase_count` | Total number of orders |
| `avg_order_value` | Mean order size |
| `days_since_last_purchase` | Recency signal |
| `campaign_responses` | Marketing engagement count |

**Label**: `purchased` — whether the customer made a purchase in the last 90 days.

---

## Running Tests

```bash
pytest tests/ -v
# With coverage:
pytest tests/ --cov=app --cov-report=html
```

---

## Environment Variables

| Variable | Description | Default |
|---|---|---|
| `SECRET_KEY` | Flask session secret | dev-key |
| `DATABASE_URL` | SQLAlchemy connection string | SQLite |
| `FLASK_ENV` | development / production | development |

---

## Contributing

1. Fork the repo
2. Create a feature branch: `git checkout -b feat/your-feature`
3. Commit and push
4. Open a pull request

---

## License

MIT – see [LICENSE](LICENSE) for details.
