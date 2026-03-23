# ridestream-data-pipeline

RideStream is an end-to-end Azure-based data engineering project that ingests batch ride data and real-time booking events, unifies them into a lakehouse architecture, and serves curated analytics-ready datasets for business users.

The project is inspired by real-world ride-hailing data platform patterns and demonstrates modern data engineering practices using Azure Data Factory, Azure Storage, Azure Event Hubs, Databricks, Delta Live Tables, and a medallion-style architecture.



## Architecture Overview

The pipeline is designed to process two types of data:

- **Batch data**: Bulk ride and mapping data stored on an HTTP server and internal sources.
- **Streaming data**: Live booking events captured from the application through Azure Event Hubs.

Both sources are landed into **Azure Data Lake Storage Gen2**, then processed in **Databricks** using streaming and batch transformations. The data is unified into a **silver-layer OBT** and then modeled into a **gold-layer star schema** for analytics.



## Business Problem

Ride-hailing companies generate both historical and live operational data. Business users need a reliable platform to:

- Track ride bookings in real time.
- Analyze driver, passenger, vehicle, and location performance.
- Maintain consistent historical records for changing dimensions.
- Expose clean fact and dimension tables for downstream reporting.

RideStream solves this by combining batch and streaming data into a unified analytics model.



## Tech Stack

- **Azure Data Factory** for orchestration and batch ingestion.
- **Azure Data Lake Storage Gen2** for raw and curated data storage.
- **Azure Event Hubs** for capturing live booking events.
- **Azure Databricks** for data processing and transformation.
- **Delta Live Tables** for declarative pipeline management.
- **Streaming tables** for incremental processing.
- **Delta Lake** for reliable storage and ACID transactions.
- **JSON-based external mappings** for reference and lookup data.



## Data Flow

### 1. Raw Ingestion
Batch files and mapping data are copied from HTTP/internal sources into Azure Data Lake using Azure Data Factory.

### 2. Streaming Capture
Live booking events are ingested through Azure Event Hubs and made available for processing in Databricks.

### 3. Unified Processing
Batch and streaming sources are combined into one unified flow using append-based processing and Databricks streaming logic.

### 4. Silver Layer
The silver layer contains the **OBT (One Big Table)**, which consolidates ride, booking, driver, passenger, vehicle, and location information into a clean analytical format.

### 5. Gold Layer
The gold layer is modeled as a **star schema** with dimension tables and a fact table for business reporting.



## Silver Layer Design

The silver layer acts as the curated transformation layer. It integrates data from:

- Batch ride datasets.
- Streaming booking events.
- External JSON mappings.

This layer creates a unified OBT that simplifies downstream modeling and improves usability for analytics and reporting.


## Gold Layer Design

The gold layer is built as a dimensional model with the following tables:

- `dim_driver`
- `dim_passenger`
- `dim_vehicle`
- `dim_booking`
- `dim_location`
- `fact_ride` 

This structure is optimized for business intelligence, dashboarding, and SQL-based reporting.



## Slowly Changing Dimension

The project implements **Slowly Changing Dimension Type 2** for the `city` dimension.

This allows the pipeline to preserve historical changes to city attributes over time while still supporting current-state analytics. It is especially useful when business attributes change and historical accuracy must be maintained.


## Key Features

- End-to-end Azure lakehouse pipeline.
- Batch and streaming data integration.
- Medallion architecture with bronze, silver, and gold layers.
- Unified OBT creation in the silver layer.
- Star schema modeling in the gold layer.
- SCD Type 2 support for historical dimension tracking.
- Declarative pipelines using Delta Live Tables.
- Scalable and production-style data engineering design.
