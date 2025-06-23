# 🧠 Machine Learning for IoT (MLoT) - Architecture Overview

This document describes an end-to-end architecture for applying **machine learning to real-time IoT sensor data**. The system integrates data collection from distributed devices via **MQTT** and **LoRa**, streams it using **Apache Kafka**, processes it for analytics and modeling, and outputs actionable insights to users and actuators.

![MLIoT](./assets/images/MLIoT.png)

---

## 📡 1. IoT Devices (Things)

The system begins with IoT-enabled devices — referred to as **"Things"** — deployed in the environment to collect sensor readings such as:

- 🌡️ **Temperature**
- 💧 **Humidity**
- ⚖️ **Pressure**

These devices can communicate data using two main technologies:

- **Direct MQTT**: Devices connect to a nearby MQTT broker and send data in lightweight publish/subscribe format.
- **LoRa Gateway**: For low-power, long-range communication, devices transmit via LoRa to a gateway, which then forwards data to an MQTT broker.

---

## 📥 2. Data Ingestion

### 🔶 MQTT Cluster

- A **cluster of MQTT brokers** receives data directly from devices or indirectly via LoRa gateways.
- MQTT enables lightweight messaging using topics (e.g., `iot-frames-model`, `iot-frames-sensor`).
- MQTT messages are forwarded to a **Kafka streaming system** for scalable processing.

### 🔶 LoRa Gateway

- LoRa devices send data to **LoRa gateways**, which aggregate and decode signals.
- The gateways act as MQTT clients, pushing messages to the cluster for further routing into Kafka.

This structure allows flexible deployment in various environments, including rural or industrial areas.

---

## 🔄 3. Streaming Cluster

### ⚙️ Apache Kafka

Kafka serves as the backbone for the streaming data pipeline:

- It ingests messages from MQTT.
- Organizes them into **topics** (e.g., `stream-frame`.
- Enables real-time consumption by **multiple downstream consumers**, including:
  - Machine learning models
  - Processing modules
  - Visualization tools

Kafka’s distributed nature ensures fault tolerance, scalability, and durability.

---

## 🤖 4. Machine Learning System

This component handles the core intelligence of the system.

### 🏗️ ML Model (Training)

- Historical sensor data from Kafka is used to **train predictive models** (e.g., regression, classification).
- This may be done in **batch mode** or **incremental (online) mode** using frameworks like:
  - `scikit-learn`, `River`, `TensorFlow`, `PyTorch`

### ⚡ Realtime Model

- Trained models are deployed to **make real-time predictions** as data streams in.
- Examples of predictions:
  - Predicting fan speed from temperature/humidity
  - Detecting abnormal pressure patterns

### 🧮 Processing

- Performs **windowing** (e.g., average values every 1 min) and **feature engineering**.
- This smooths out noise and prepares the data for ML inference.
- Real-time stream processors like Kafka Streams or Flink can be used here.

### 🚀 ML Model (Deploy)

- Trained and validated models are deployed into production for live inference.
- Predictions are output to Kafka or stored in databases for further use.

---

## 💾 5. Output & Visualization

### 🗃️ Data Store

- Stores processed sensor data and ML predictions.
- Common backends: PostgreSQL, InfluxDB, MongoDB, or time-series databases.

### 📊 Visualization

- Dashboards are built to monitor sensor trends, prediction outcomes, and system health.
- Tools like **Grafana**, **Kibana**, or custom web apps are used.

---

## 👤 6. User Interaction

End users — engineers, system operators, or administrators — can:

- Monitor real-time visualizations.
- Be alerted of anomalies or threshold breaches.
- Take action based on insights, such as:
  - 💡 Turning on/off lights
  - 🔧 Activating motors or valves
  - 🧠 Triggering further processing workflows

The goal is to **automate decisions** or **assist human operators** in responding promptly.

---

## 🔁 🔄 Data Flow Summary

```plaintext
1. IoT Devices → MQTT or LoRa → MQTT Broker
2. MQTT → Kafka (Streaming Ingestion)
3. Kafka → Processing → ML Model (Training & Inference)
4. ML Predictions → Data Store → Visualization
5. User sees output → Takes action (manual or automatic)


## Containers
1. Server [https://github.com/hanattaw/Iot-class-2025-server](https://github.com/hanattaw/Iot-class-2025-server)
2. Gateway [https://github.com/hanattaw/Iot-class-2025-gateway](https://github.com/hanattaw/Iot-class-2025-gateway)
3. Sensor
4. Batch ML
5. Online ML