# 🐾 PET TRACKING – Emotional and Behavioral Analytics System

### Big Data & AI Project | AWS | Streaming | ML | Power BI
[🇬🇧 English](./README.md) | [🇪🇸 Español](./README.es.md)

---

## 🚀 Overview

**Pet Tracking** is a data analytics system designed to **monitor, interpret, and anticipate animal well-being** based on physiological signals, behavior, and sound.  
The project is conceived as a **full Big Data system**, combining **batch and streaming ingestion**, distributed processing, **Machine Learning integrated directly into the data warehouse**, and decision-oriented visualization.

This is not an isolated model.  
It is an **end-to-end data architecture**, designed to scale and operate in real-world environments.

---

## 🎯 What the system does

- Processes heterogeneous data (sensors, audio, activity, events).
- Detects behavioral patterns and emotional states.
- Identifies risk situations using rules and Machine Learning.
- Exposes results through analytical queries and dashboards.
- Reduces latency between event detection and alert generation.

---

## 🧠 System architecture (high level)

```plaintext
[IoT Devices / Sensors / Audio / Reports]
↓
Ingestion Layer
├── AWS Glue + S3 (Batch)
├── Kinesis + Lambda + Firehose (Streaming)
↓
Data Lake and Analytical Storage
Amazon S3 / Athena / Redshift
↓
Processing and Machine Learning
EMR (Spark) / Redshift ML
↓
Decision Layer
Power BI Dashboards
```

---

## 📥 Data sources
| Source | Data |
|--------|------|
| Wearables and sensors | Heart rate, temperature, activity, GPS |
| Microphones | Vocalizations and sound patterns |
| Cameras | Movement and posture (conceptual extension) |
| Human reports | Emotional labels (joy, anxiety, pain, hunger, etc.) |

---

## 🔄 Data ingestion
Batch ingestion
Source: Periodic reports and historical data.

AWS services:

Amazon S3

AWS Glue Studio

Glue Data Catalog

Glue Data Quality

Key operations:

Schema normalization

Type enforcement

Data quality validations

Preparation for reproducible pipelines

Streaming ingestion (real time)
Source: IoT devices emitting events continuously.

AWS services:

Amazon Kinesis Data Streams

AWS Lambda (JSON transformation and enrichment)

Amazon Firehose (delivery to S3)

Format:

Parquet + Snappy

Partitioned by year / month / day

---

## 🧱 Data Lake and storage design
Main bucket:
s3://pet-tracking-data-bucket/

```plaintext
Copiar código
├── raw/
│   ├── batch/
│   └── stream/
├── processed/
├── firehose-output/
├── warehouse/
│   ├── athena/
│   └── redshift/
├── dashboards/
├── logs/
├── archive/
└── s3-management/
```
---

## Lifecycle policies
| Path | Action | Retention |
|------|--------|-----------|
| raw/batch/ | Glacier | 30 days |
| raw/stream/ | Delete | 7 days |
| processed/ | Delete | 90 days |
| firehose-output/ | Delete | 60 days |

---

## ⚙️ Analytical processing
AWS EMR (Apache Spark)
Distributed processing for:

Data cleaning

Aggregations per pet

Activity classification

Anomaly and alert detection

PySpark jobs executed on EMR clusters.
Results persisted to S3 for downstream consumption.

Athena
Ad-hoc SQL queries over partitioned Parquet datasets.

Used for exploration, validation, and lightweight analytics.

Example:

```sql
SELECT emotion, COUNT(*) AS freq
FROM pet_behavior_parquet
GROUP BY emotion
ORDER BY freq DESC;
```

---

## 🤖 Machine Learning inside the warehouse
Redshift + Redshift ML
Unsupervised K-Means model trained directly in Redshift.

Features used:

age

heart_rate_bpm

activity_steps

gps_lat, gps_lon

The model is used directly in SQL queries to assign behavioral clusters.

No external ML pipelines are required.

This enables analytics and Machine Learning in a single layer.

---

## 📊 Visualization and decision-making
Power BI dashboards

Emotional state

Activity level

Behavior distribution

Risk indicators and alerts

Designed for:

Veterinarians

Caregivers

Non-technical users

The focus is on decision-making, not technical inspection.

---

## 🧩 Why this is not a toy project
Hybrid batch + streaming architecture.

Explicit data quality validations.

Clear separation between raw, processed, and analytical layers.

Distributed processing with Spark.

Machine Learning embedded in the warehouse, not notebooks.

Design focused on scalability, governance, and reproducibility.

---

## 🛠️ AWS services used
| Layer | Services |
|-------|----------|
| Batch ingestion | S3, Glue Studio, Glue Catalog, Glue Data Quality |
| Streaming ingestion | Kinesis, Lambda, Firehose |
| Storage | S3, Athena, Redshift |
| Processing | EMR (Spark), Redshift ML |
| Visualization | Power BI |

---

## 🧪 Next steps
Multimodal Deep Learning (CNN/LSTM) for richer emotion detection.

Integration with Amazon SageMaker for ML orchestration.

Mobile application with real-time notifications.

Use of AWS IoT Core for direct device management.

Concept drift detection and model retraining.

---

## 📁 Project structure
```plaintext
Copiar código
pet-tracking-project/
├── glue/
├── kinesis/
├── emr/
├── warehouse/
├── dashboards/
├── logs/
└── docs/
```

---

## 📚 License
MIT License.
Free to use, modify, and distribute with attribution.

---

© 2025 – Pet Tracking | Big Data & AI Analytics System
