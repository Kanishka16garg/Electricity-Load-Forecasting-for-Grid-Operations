# ⚡ Electricity Load Forecasting System – API Documentation

## 🌐 Base URL
/api/v1
---

## 🔐 Authentication APIs

| Method | Endpoint          | Role        | Description                                   | Request Body              | Response              |
|--------|------------------|------------|-----------------------------------------------|---------------------------|-----------------------|
| POST   | /auth/register   | Public     | Register new user (operator/analyst)          | name, email, password     | JWT token + role      |
| POST   | /auth/login      | Public     | User login                                    | email, password           | JWT token + role      |
| GET    | /auth/me         | User/Admin | Get logged-in profile                         | —                         | User profile          |

---

## ⚡ Data Ingestion APIs

| Method | Endpoint            | Role  | Description                                   | Request Body | Response         |
|--------|--------------------|------|-----------------------------------------------|--------------|------------------|
| POST   | /data/upload       | Admin | Upload electricity dataset (CSV)              | CSV file     | Upload status    |
| GET    | /data/meters       | User  | Get available meter IDs                       | —            | List of meters   |
| GET    | /data/meter/{id}   | User  | Get historical load data for meter            | —            | Time-series data |

---

## 📊 Forecast APIs (Core Feature)

| Method | Endpoint                      | Role | Description                                | Request Body          | Response                |
|--------|------------------------------|------|--------------------------------------------|-----------------------|-------------------------|
| POST   | /forecast/generate           | User | Generate load forecast for a meter         | meter_id, horizon     | Forecast values         |
| GET    | /forecast/{meter_id}         | User | Get latest forecast                        | —                     | Forecast + timestamps   |
| GET    | /forecast/history/{meter_id} | User | Get past forecasts                         | range=24h/7d          | Forecast history        |

---

## 🤖 ML / Model APIs

| Method | Endpoint                  | Role  | Description                                  | Request Body | Response                |
|--------|--------------------------|-------|----------------------------------------------|--------------|-------------------------|
| POST   | /ml/train                | Admin | Train ARIMA model for a meter                | meter_id     | Training status         |
| POST   | /ml/retrain              | Admin | Retrain model with latest data               | meter_id     | Updated metrics         |
| GET    | /ml/modelInfo/{meter_id} | Admin | Get model details                            | —            | AIC, BIC, params        |
| GET    | /ml/metrics/{meter_id}   | Admin | Get evaluation metrics                       | —            | MAE, sMAPE              |

---

## 📈 Visualization & Analytics APIs

| Method | Endpoint                          | Role  | Description                               | Request Body      | Response        |
|--------|-----------------------------------|-------|-------------------------------------------|-------------------|-----------------|
| GET    | /analytics/trend/{meter_id}       | User  | Load trend data                           | range=daily/weekly| Trend dataset   |
| GET    | /analytics/comparison/{meter_id}  | User  | Actual vs forecast comparison             | —                 | Graph data      |
| GET    | /analytics/anomaly/{meter_id}     | Admin | Detect anomalies in load                  | —                 | Anomaly points  |
| GET    | /analytics/export/{meter_id}      | User  | Export forecast data                      | —                 | CSV file        |

---

## 🗄️ Storage APIs

| Method | Endpoint                       | Role  | Description                    | Request Body | Response          |
|--------|--------------------------------|-------|--------------------------------|--------------|-------------------|
| GET    | /storage/forecasts/{meter_id}  | Admin | Get stored forecasts          | —            | Forecast records  |
| DELETE | /storage/forecast/{id}         | Admin | Delete forecast entry         | —            | Status            |

---

## 🔄 Batch Processing APIs

| Method | Endpoint          | Role  | Description                             | Request Body   | Response         |
|--------|------------------|-------|-----------------------------------------|----------------|------------------|
| POST   | /batch/forecast  | Admin | Run forecast for multiple meters        | meter_ids[]    | Batch ID         |
| GET    | /batch/{batch_id}| Admin | Get batch results                       | —              | Forecast summary |
