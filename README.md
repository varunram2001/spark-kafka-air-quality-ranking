# OPPE2: Air Quality Monitoring and Ranking Pipeline: Real-Time Structured Streaming

## 1. Project Overview

This project implements a comprehensive, end-to-end data pipeline on Google Cloud Platform (GCP) to process real-time environmental data. The solution performs crucial data quality checks, **8-hour windowed aggregation**, and **dynamic global ranking** using **Apache Kafka** and **Apache Spark Structured Streaming**.

The final architecture provides a robust, fault-tolerant system capable of handling high-volume time-series analysis and delivering near-real-time results.

## 2. Problem Statement & Objectives

### 2.1 Problem Statement

Analyze a global air quality dataset (`globalAirQuality.csv`) containing hourly readings for multiple cities and various pollutants (PM2.5, NO2, O3, AQI, etc.). The core requirement is to **simulate a continuous data stream**, perform necessary data cleaning and imputation, and calculate a dynamic ranking of cities based on their air quality over rolling 8-hour windows.

### 2.2 Project Objectives (What We Did)

| Objective | Implementation Details |
| :--- | :--- |
| **Data Quality & Imputation** | Implemented a PySpark Producer that performs **forward-filling (imputation)** on missing pollutant values using Spark Window functions ordered by time and partitioned by city. |
| **Decoupling and Streaming Source** | Used **Apache Kafka** (on a Compute Engine VM) as the durable message broker, decoupling the Producer from the Consumer. |
| **Windowed Aggregation** | Implemented a Structured Streaming Consumer that groups data into an **8-hour tumbling window** to calculate required health metrics: <ul><li>**V1 (Max AQI):** Maximum Air Quality Index observed in the window.</li><li>**V2 (Sum of Pollutants):** Total sum of all major pollutant concentrations in the window.</li></ul> |
| **Dynamic Global Ranking** | Used the Spark **`foreachBatch`** sink to apply a final **global rank** across all cities whose 8-hour window completed, ranking them primarily by V1 (Max AQI, ASC) and secondarily by V2 (Sum of Pollutants, ASC). |
| **Fault Tolerance** | Configured the Streaming consumer with a **GCS Checkpoint Location** to ensure the job can resume processing instantly if stopped or failed. |

## 3. Technology Stack

| Component | Technology Used | Role in Pipeline |
| :--- | :--- | :--- |
| **Cloud Infrastructure** | Google Cloud Platform (GCP) | Provides resources: Compute Engine (VM), Dataproc (Spark Cluster), Cloud Storage (GCS). |
| **Message Broker** | Apache Kafka / Zookeeper | High-throughput buffer for the cleaned data stream. |
| **Processing Engine** | Apache Spark 3.3.0 (on Dataproc) | Used for scalable batch processing (Producer) and fault-tolerant stream processing (Consumer). |

## 4. Submission Files Included

| File Name | Description |
| :--- | :--- |
| `CommandsUsed_21f1001490_OPPE2.docx` | Complete, ordered list of all `gcloud`, `gsutil`, and shell commands used for infrastructure setup and execution. |
| `StructuredStreaming_Output_OPPE2.txt` | Text output confirming the consumer job successfully generated the final **Windowed Ranking Table**. |
| `kafka_zookper_running_success.txt` | Console output confirming Kafka and Zookeeper services status on the VM. |
| `batch_producer.py` | PySpark script for data cleaning, imputation, and pushing data to Kafka. |
| `streaming_consumer.py` | PySpark script for reading from Kafka, applying the 8-hour window, and global ranking. |
| `globalAirQaulity.csv` | The raw dataset used as the source for the pipeline. |
| `ScreenshotXX_...png` (11 files) | Visual evidence of infrastructure setup, Kafka status, and live output. |
| **README.md** | This documentation file. |

## 5. Screencast Link 

**https://drive.google.com/file/d/1_IB9Vr-SJ2WVWyNpV7feJbiWv9C5ixWv/view?usp=sharing**

Open access given for IITM Student Mail ID.

## 6. LLM Usage Link

**https://gemini.google.com/share/17af69fd5e5c**