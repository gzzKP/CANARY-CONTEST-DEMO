# Industrial Treatment Furnace Demo

### Scalable OT–IT Analytics with MQTT and Canary Labs

## Overview

This demo showcases a scalable industrial analytics architecture for **thermal treatment furnaces**, built on **MQTT**, **Canary Labs**, and **AXIOM**.

The objective is to demonstrate how raw OT data from PLCs and field devices can be transformed into **structured, reliable, and reusable analytics** using an **Asset Model–driven approach**, without manual tag-to-asset mapping.

---

## Architecture Summary

**Data Flow:**

1. PLCs and field systems publish process variables to a central **MQTT Broker**
2. **Canary MQTT Collector** subscribes to topics and historizes the data
3. Signals are auto-modeled using **naming conventions and REGEX rules**
4. Assets, KPIs, events, and dashboards are inherited through the **Asset Model**
5. **AXIOM** provides operational and analytical visualization

**Key Principles:**

* OT / IT separation
* Event-driven ingestion
* Scalability by design
* Built-in data quality management

---

## Asset Model Structure

The demo uses a hierarchical asset model:

* **Area / Plant**

  * **Furnace (Estufa)**

    * **Burner (Quemador)**

      * **Flame (Flama)**

Assets are created automatically from MQTT topic names using REGEX rules.
No manual tag-to-asset mapping is required.

---

## Views and Navigation

### Process View (Plant Level)

**Path:** `MEX_AXIOM.Monterrey`

* Overview of all active furnaces
* Batch in process
* Operating time
* Target vs actual temperature
* Gas consumption
* Real-time comparison across furnaces

### Furnace View

**Path:** `QUEMADOR.Estufa1`

* Global furnace status
* Individual burner states
* Fault, ignition, cooling, low/high flame
* Temporal state evolution with temperature correlation

### Events View

**Tab:** `EVENTOS`

* Automatically detected furnace cycles
* Cycle duration and validation
* Fault occurrence by burner
* No external scripts required

### Faults View

**Tab:** `FALLAS`

* Fault analysis by:

  * Day / week / month
  * Furnace
  * Burner
* Identification of recurring issues and critical assets

### Burner View

**Path:** `QUEMADOR.Estufa1.Quemador1`

* ON/OFF cycling
* Flame stability
* Temperature correlation
* Historical fault analysis
* Dashboard inherited from burner asset type

---

## KPIs and Analytics

KPIs are defined at the **asset type level** and inherited automatically:

* Daily gas consumption (Furnace)
* Burner stability (ON/OFF cycling)
* Fault counts and rates
* Cycle duration metrics

All calculations include **data quality validation** to prevent KPIs from using invalid or missing data.

---

## Recreating the Project: JSON Import Guide

### Overview

This repository includes JSON configuration files that contain all **Events** and **Calculations** definitions. These can be directly imported into **Canary Admin** to recreate the entire project setup without manual configuration.

### JSON Files

The configuration files are located in the `CALCULOS_JSON/` directory:

* **`CALCULOS_JSON/EVENTOS`** — Event definitions (furnace cycle detection, fault tracking)
* **`CALCULOS_JSON/ESTABILIDAD_HORNOS`** — Burner stability calculations and KPI formulas

### How to Import

#### Step 1: Access Canary Admin
- Log into your Canary Labs instance
- Navigate to **Admin Console** → **Jobs** or **Calculations**

#### Step 2: Import Events
1. Open the **Events** section in Canary Admin
2. Click **Import** or **Load Configuration**
3. Select the `CALCULOS_JSON/EVENTOS` file
4. Review the event definitions (cycle start/stop conditions, properties, etc.)
5. Click **Apply** to create the events

#### Step 3: Import Calculations
1. Open the **Calculations** or **KPIs** section
2. Click **Import** or **Load Configuration**
3. Select the `CALCULOS_JSON/ESTABILIDAD_HORNOS` file
4. Review the calculation expressions and trigger settings
5. Click **Apply** to deploy the calculations

#### Step 4: Verify
- Navigate to the **EVENTOS** and **Fallas** tabs in AXIOM
- Confirm that all events are being detected
- Verify that KPI tags are being populated with calculated values

### Replicating the Project

To replicate this project in a different environment:

1. **Download** the JSON files from this repository
2. **Import** them into your Canary Admin instance following the steps above
3. **Customize** asset paths and MQTT topics to match your infrastructure
4. **Connect** your MQTT broker and verify data ingestion
5. **Export** your updated configurations back to JSON for version control and disaster recovery

### Configuration Structure

Each JSON file contains an array of job objects with the following key sections:

- **Trigger**: Defines when the calculation runs (Periodic, Subscription-based, etc.)
- **Asset**: Specifies the asset path and type
- **Expressions**: Contains the calculation logic (using Canary expression language)
- **Tags**: Output tag paths where results are written
- **Properties**: Custom metadata and event properties

---

## Key Benefits Demonstrated

* No manual dashboard duplication
* Automatic scaling when new furnaces or burners appear
* Consistent KPIs across plants
* Fast root cause analysis
* Data-driven maintenance decisions
* Enterprise-ready architecture

---

## Future Improvements & Roadmap

### 1. **UI/UX Refinement**

* **Cleaner View Structure**: Reorganize dashboard layouts for improved readability
  * Consolidate redundant information panels
  * Implement responsive design for mobile/tablet access
  * Add collapsible sections for advanced metrics
* **Unified Navigation**: Create breadcrumb navigation across plant → furnace → burner hierarchy
  
### 2. **MQTT Data Publishing for Ecosystem Integration**

* **Consumer-Ready Topics**: Define standardized MQTT output topics for downstream consumers
  * `analytics/kpi/<plant>/<furnace>/<metric>` — Real-time KPI stream
  * `analytics/alerts/<plant>/<furnace>/<severity>` — Alert propagation
  * `analytics/events/<plant>/<furnace>/cycle` — Cycle completion events
* **Data Format Standardization**: Publish normalized JSON payloads with schema versioning
* **Quality Indicators**: Include data quality scores in published messages for consumer confidence
* **Third-Party Integration**: Enable external systems (MES, ERP, predictive maintenance tools) to subscribe

### 3. **Geographic and Operational Scaling**

* **Multi-Site Support**: Extend from single plant (Monterrey) to multiple geographic regions
  * Support for different time zones and regulatory requirements
  * Aggregated dashboards across sites with site-specific drill-down
* **Additional Equipment Types**: Extend asset model beyond furnaces to:
  * Compressors, coolers, and heat exchangers
  * Material handling systems
  * Emission control units
* **High-Volume Data**: Optimize data ingestion pipeline for 100s of furnaces and 1000s of signals
  * Data compression and archival strategies
  * Time-series database optimization

### 4. **Analytics Enhancement**

* **Predictive Maintenance**: ML-driven fault prediction models
  * Historical trend analysis for early warning indicators
  * Anomaly detection using statistical baselines
* **Energy Optimization**: Advanced consumption analytics
  * Gas efficiency trends
  * Comparative analysis across furnaces and shifts
* **Compliance Reporting**: Automated regulatory reports
  * Emissions tracking
  * Process documentation for certifications

### 5. **Operational Excellence**

* **Alert Routing**: Intelligent alerting with escalation logic
  * Severity-based routing to maintenance/operations teams
  * Integration with ticketing systems (Jira, ServiceNow)
* **Historical Playback**: Reconstruct past events for training and forensics
* **Asset Lifecycle Tracking**: Monitor equipment age, maintenance history, and replacement scheduling

### 6. **Developer Experience**

* **API Documentation**: REST/GraphQL APIs for programmatic access to analytics
* **Configuration as Code**: Template-based asset model deployment for rapid replication
* **Docker/Kubernetes Support**: Containerized deployment for portability and orchestration

---

## Next Steps (Priority Order)

1. **Phase 1**: Implement cleaner UI structure and MQTT output topics
2. **Phase 2**: Extend to additional plants and furnace types
3. **Phase 3**: Develop predictive maintenance models
4. **Phase 4**: Build compliance and regulatory reporting module
