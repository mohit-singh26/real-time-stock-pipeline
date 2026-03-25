# 🚀 Real-Time Stock Market Data Pipeline on AWS

## 📌 Overview

I built this project while exploring trading setups and trying to understand how tools like Chartink actually work behind the scenes.

Instead of just using screeners, I designed a **data pipeline that simulates how raw stock data is ingested, processed, stored, and analyzed** using AWS services.

---

## 🧠 Problem Statement

Stock screeners and analytics tools look simple from outside, but internally they rely on:

* Continuous data ingestion
* Real-time processing
* Fast querying systems
* Historical data storage

This project is a **basic version of that system architecture**.

---

## 🏗️ Architecture

![Architecture](architecture.png)

---

## ⚙️ Tech Stack

* Python (yfinance)
* AWS Kinesis (Data Streaming)
* AWS Lambda (Processing)
* Amazon S3 (Data Lake)
* Amazon DynamoDB (Low-latency storage)
* AWS Glue (Schema + ETL)
* Amazon Athena (SQL queries)
* Amazon SNS (Alerts)

---

## 🔄 Data Flow (Step-by-Step)

1. Python script fetches stock data using yfinance
2. Data is pushed into AWS Kinesis Data Stream
3. Lambda function processes data:

   * Price change calculation
   * % change calculation
   * Basic anomaly detection (>5%)
4. Processed data stored in DynamoDB
5. Raw data stored in S3 (partitioned by symbol + timestamp)
6. AWS Glue creates schema from S3 data
7. Athena runs SQL queries on historical data
8. SNS used for alert simulation

---

## 📊 Sample Queries (Athena)

### 🔹 Top 5 Stocks by Price Change

```sql
SELECT symbol, price, previous_close,
       (price - previous_close) AS price_change
FROM stock_data_table
ORDER BY price_change DESC
LIMIT 5;
```

---

### 🔹 Average Trading Volume

```sql
SELECT symbol, AVG(volume) AS avg_volume
FROM stock_data_table
GROUP BY symbol;
```

---

### 🔹 Detect Anomalies (>5% move)

```sql
SELECT symbol, price, previous_close,
       ROUND(((price - previous_close) / previous_close) * 100, 2) AS change_percent
FROM stock_data_table
WHERE ABS(((price - previous_close) / previous_close) * 100) > 5;
```

---

## ⚠️ Limitations (Honest Reality)

* Not true real-time (data fetched every ~30 seconds)
* Uses yfinance (not exchange-grade data)
* Basic anomaly logic (not production-level ML)
* SNS alerts are simulated
* Built for learning system design, not production trading

---

## 🎯 Key Learnings

* How streaming systems work (Kinesis)
* Event-driven architecture using Lambda
* Difference between hot path (real-time) vs cold path (analytics)
* Data lake design using S3 + Glue + Athena
* How real-world stock analytics systems are structured

---

## 🚀 Future Improvements

* Use real-time market data APIs (WebSockets)
* Add Kafka instead of Kinesis
* Implement advanced anomaly detection (ML models)
* Build dashboard (Streamlit / React)
* Automate alerts with real signals

---

## 👨‍💻 Author

**Mohit Singh**
Built as part of Data Engineering project + trading system exploration

---

## ⭐ Final Note

This is not a production system — it's a **learning-focused, system design project** that helped me understand how real-world data pipelines work behind the scenes.
