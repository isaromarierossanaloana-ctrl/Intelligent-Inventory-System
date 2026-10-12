# Intelligent Inventory System

A Flask and MongoDB inventory application for product and stock management,
demand forecasting, stock-risk classification, analytics, and replenishment
recommendations.

## Requirements

- Python 3.10 or newer
- MongoDB 6 or newer, either installed locally or started with Docker Compose

## Setup

```powershell
py -m venv .venv
.\.venv\Scripts\Activate.ps1
pip install -r requirements-dev.txt
Copy-Item .env.example .env
docker compose up -d mongodb
flask --app run.py run
```

Open <http://127.0.0.1:5000>. The MongoDB URI, database name, and model
directory can be changed in `.env` (or set as environment variables).
MongoDB connection errors are shown by the API health endpoint and are not
silently replaced with an in-memory database.

## Inventory rules

- A Critical product has no stock on hand.
- A Low Stock product has stock at or below its reorder level.
- An Overstock product has more than three times its reorder level.
- Other products are classified as OK.
- A recommendation covers predicted demand for the selected forecast period
  plus the product's reorder level, less current stock; recommended quantities
  never fall below zero.
- Demand is measured from stock-out transactions. Daily dates without recorded
  sales count as zero demand.

## API overview

- `GET /api/health` — MongoDB connectivity
- `GET, POST /api/products` — list/search and register products
- `GET, PUT, DELETE /api/products/<product_id>` — read, update, and delete
- `POST /api/transactions` — record stock-in or stock-out movements
- `GET /api/history` — inventory movement history
- `POST /api/predictions` — forecast demand and recommend replenishment
- `GET /api/predictions` — review saved forecasts
- `GET /api/analytics` — dashboard counts, stock statuses, value, and demand
- `POST /api/models/train` — train and evaluate the linear-regression model
- `GET /api/models` — review saved model evaluation runs
- `GET /api/reports` — inventory report data

Run tests with:

```powershell
pytest
```
