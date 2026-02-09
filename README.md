# Industrial Treatment Furnace Demo

### Scalable OT–IT Analytics with MQTT and Canary Labs

## Overview

This demo showcases a scalable industrial analytics architecture for **thermal treatment furnaces**, built on **MQTT**, **Canary Labs**, and **AXIOM**.

The objective is to demonstrate how raw OT data from PLCs and field devices can be transformed into **structured, reliable, and reusable analytics** using an **Asset Model–driven approach**, without manual signal configuration or duplicated dashboards.

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

## Key Benefits Demonstrated

* No manual dashboard duplication
* Automatic scaling when new furnaces or burners appear
* Consistent KPIs across plants
* Fast root cause analysis
* Data-driven maintenance decisions
* Enterprise-ready architecture
