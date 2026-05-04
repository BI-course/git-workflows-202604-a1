#Introduction
Data is the foundational raw material of Business Intelligence (BI).
Before any analysis, reporting, or decision-support system can be
built, organisations must first identify, understand, and classify
the systems from which their data originates. A data source is any
system, file, service, or device that generates or stores data that
can be extracted for analytical purposes.

In modern enterprises, data is not generated from a single location.
It flows from transactional databases, spreadsheets, third-party
APIs, IoT sensors, and real-time event streams — each with different
structures, update frequencies, and reliability characteristics.
Understanding these source types is a prerequisite for designing
pipelines that are both technically sound and legally compliant.

This document reviews the primary categories of data sources used
in Business Intelligence architectures, evaluates their suitability
for analytical workloads, and provides a structured reference for
pipeline design decisions in subsequent lab work.

#Literature Review
Early BI systems in the 1980s and 1990s drew exclusively from
internal relational databases — typically OLTP systems running on
mainframes or early client-server architectures (Inmon, 1992).
Inmon's foundational definition of a data warehouse as a
"subject-oriented, integrated, non-volatile, and time-variant
collection of data" assumed that source data came primarily from
structured, internal transactional systems.

Kimball and Ross (2013) later formalised the dimensional modelling
approach, which similarly assumed well-structured relational sources.
However, they acknowledged that flat files — particularly CSV exports
from legacy systems — were a practical and common source type that
pipeline designers had to accommodate.

## 2.2 The Expansion of Source Types

The proliferation of web-based applications in the 2000s introduced
APIs as a primary data source. Fielding's (2000) dissertation
formalising REST architectural constraints established the foundation
for the API economy that followed. By the 2010s, organisations were
consuming data from hundreds of external SaaS APIs — CRMs, marketing
platforms, payment processors — making API integration a core
competency of data engineering (Reis & Housley, 2022).

The emergence of IoT and distributed systems introduced streaming
data sources. Kleppmann (2017) describes streaming data as a
fundamentally different paradigm: rather than querying a database
at a point in time, the system reacts to a continuous flow of events.
Apache Kafka, introduced by LinkedIn in 2011 and open-sourced
thereafter, became the dominant infrastructure for streaming data
ingestion at scale.

## 2.3 Source Systems and Data Quality

Redman (1996) established that data quality problems originate
predominantly at the source — not in the warehouse or reporting
layer. This insight has significant implications for BI design:
the characteristics of a source system (its schema stability,
update frequency, and error handling) directly determine the
complexity and reliability of the downstream pipeline.

#Theoretical Framework
The theoretical basis for understanding data sources in BI is the
**Source-to-Target (S2T) model**, which conceptualises a data
pipeline as a directed graph from origin systems to analytical
targets. Each node in the graph is either a source, a
transformation step, or a target. The properties of each source
node — structure, latency, volume, and governance — constrain
the design of the entire pipeline.

## 3.2 Data Source Classification Dimensions

Data sources can be theoretically classified across four dimensions:

### Dimension 1 — Structure
| Level           | Description                                 | Example              |
|-----------------|---------------------------------------------|----------------------|
| Structured      | Fixed schema, relational, typed columns     | MySQL, PostgreSQL    |
| Semi-structured | Schema is implicit, self-describing         | JSON, XML, Parquet   |
| Unstructured    | No predefined schema                        | PDFs, images, emails |

### Dimension 2 — Latency
| Type       | Update Frequency          | Suitable For                   |
|------------|---------------------------|--------------------------------|
| Batch      | Hours to days             | Historical reporting           |
| Micro-batch| Minutes                   | Near-real-time dashboards      |
| Streaming  | Milliseconds to seconds   | Real-time alerting and ML      |

### Dimension 3 — Origin
- **Internal sources:** Generated within the organisation
  (transactional DBs, ERP systems, HR platforms)
- **External sources:** Generated outside the organisation
  (third-party APIs, open government datasets, market data feeds)

### Dimension 4 — Access Method
| Method              | Description                                        |
|---------------------|----------------------------------------------------|
| Direct DB connection| JDBC/ODBC connection to the source database        |
| File export         | Scheduled CSV/JSON/Parquet file drops to storage   |
| API polling         | Periodic HTTP requests to a REST or GraphQL API    |
| Event subscription  | Consuming a message queue or event stream          |
| Change Data Capture | Reading the database transaction log (e.g. Debezium)|

## 3.3 The CAP Theorem and Source Reliability

Brewer's CAP Theorem (2000) — which states that a distributed system
can guarantee at most two of Consistency, Availability, and Partition
tolerance simultaneously — has direct implications for source system
reliability. An OLTP database optimised for consistency may be
temporarily unavailable during failover, causing gaps in extracted
data. Pipeline designers must account for the theoretical limitations
of their source systems when designing extraction strategies.

#Empirical Framework
4.1 Primary Source Types in Practice

The following four categories represent the empirically dominant
data source types in enterprise BI environments, drawn from
industry surveys and practitioner literature.

---

### 4.1.1 Transactional Databases (OLTP Systems)

**Definition:** Online Transaction Processing systems are
operational databases designed to record and manage day-to-day
business transactions with high concurrency and data integrity.

**Technical Characteristics:**
- Highly normalised schema (typically Third Normal Form — 3NF)
- Row-oriented storage optimised for INSERT, UPDATE, DELETE
- ACID compliance (Atomicity, Consistency, Isolation, Durability)
- Indexed for fast point lookups, not full-table analytical scans

**Common Systems:**

| System             | Vendor      | Typical Use Case               |
|--------------------|-------------|--------------------------------|
| MySQL / MariaDB    | Oracle / OSS| Web applications, e-commerce   |
| PostgreSQL         | OSS         | General-purpose, GIS, JSONB    |
| Microsoft SQL Server| Microsoft  | Enterprise ERP, finance systems|
| Oracle Database    | Oracle      | Banking, telecoms, insurance   |
| IBM Db2            | IBM         | Mainframe financial systems    |

**Extraction Methods:**
- **Full extraction:** Dump the entire table on a schedule (simple
  but expensive at scale)
- **Incremental extraction:** Extract only rows modified since the
  last run, using a `updated_at` timestamp column
- **Change Data Capture (CDC):** Read the database transaction log
  (WAL in PostgreSQL, binlog in MySQL) to capture every INSERT,
  UPDATE, and DELETE in real time using tools like Debezium

**BI Role:** The primary source of an organisation's operational
ground truth — sales transactions, customer records, inventory
movements, and financial ledger entries.

---

### 4.1.2 Flat Files

**Definition:** Data stored in discrete files on a filesystem or
object storage, not managed by a live database engine.

**Common Formats:**

| Format  | Structure      | Encoding  | Typical Use Case                  |
|---------|----------------|-----------|-----------------------------------|
| CSV     | Tabular        | Text      | Legacy exports, cross-system data |
| JSON    | Hierarchical   | Text      | API response caches, NoSQL dumps  |
| XML     | Hierarchical   | Text      | Enterprise system integration     |
| Parquet | Columnar       | Binary    | Big data lake storage, analytics  |
| Avro    | Row-based      | Binary    | Kafka message serialisation       |
| Excel   | Tabular        | Binary    | Finance, HR, manual data entry    |

**Practical Considerations:**
- CSV files frequently contain encoding errors (UTF-8 vs Windows-1252),
  inconsistent delimiters, and missing headers — requiring defensive
  parsing logic
- Excel files may contain merged cells, formula references, and
  multiple sheets, requiring specialised parsing (e.g. openpyxl,
  pandas `read_excel`)
- Parquet is the preferred format for inter-system data exchange in
  modern data platforms due to its columnar compression and schema
  enforcement via Apache Arrow

**BI Role:** Most commonly used for one-time historical data imports,
third-party data deliveries, and as intermediate staging files in
ETL pipelines.

---

### 4.1.3 APIs (Application Programming Interfaces)

**Definition:** APIs expose data from external or internal services
over a network using standardised protocols, most commonly HTTP/REST
or GraphQL.

**API Types Relevant to BI:**

| API Type    | Protocol     | Response Format | Example                        |
|-------------|--------------|-----------------|--------------------------------|
| REST        | HTTP         | JSON / XML      | Stripe, Salesforce, M-Pesa     |
| GraphQL     | HTTP         | JSON            | GitHub, Shopify                |
| SOAP        | HTTP / XML   | XML             | Legacy banking and ERP systems |
| gRPC        | HTTP/2       | Protobuf        | Internal microservices         |
| Webhook     | HTTP (push)  | JSON            | Stripe events, GitHub events   |

**Authentication Mechanisms:**
- **API Key:** Simple token passed in request headers
- **OAuth 2.0:** Delegated authorisation — the standard for
  consuming user-specific data from SaaS platforms
- **JWT (JSON Web Tokens):** Stateless authentication for
  internal service-to-service communication
- **mTLS:** Mutual TLS — used in high-security environments
  such as banking APIs

**Rate Limiting:** APIs impose rate limits (e.g. 1,000 requests
per hour) that constrain how frequently data can be extracted.
Pipeline schedulers must implement exponential backoff and
respect `Retry-After` response headers to avoid being blocked.

**BI Role:** Essential for pulling data from SaaS platforms that
do not expose a direct database connection — CRMs, marketing
automation, payment gateways, and logistics platforms.

---

### 4.1.4 Streaming Data Sources

**Definition:** Continuous, unbounded sequences of events generated
by applications, sensors, or user interactions, consumed in real
time rather than in discrete batches.

**Streaming Infrastructure:**

| Platform        | Provider  | Key Characteristic                       |
|-----------------|-----------|------------------------------------------|
| Apache Kafka    | OSS       | High-throughput, durable, partitioned    |
| AWS Kinesis     | Amazon    | Managed, serverless option available     |
| Google Pub/Sub  | Google    | Global, push and pull subscription       |
| Azure Event Hubs| Microsoft | Kafka-compatible managed service         |
| MQTT Broker     | OSS       | Lightweight, designed for IoT devices    |
| Apache Pulsar   | OSS       | Multi-tenancy, geo-replication           |

**Streaming Concepts:**
- **Topic / Stream:** A named, ordered sequence of events
- **Producer:** The application emitting events to the stream
- **Consumer / Consumer Group:** The pipeline reading events
- **Offset:** The position of a consumer within a topic — enables
  replay of historical events
- **Event Schema:** Typically enforced via Apache Avro +
  a Schema Registry to prevent breaking changes

**Processing Frameworks:**
- **Apache Flink:** Stateful stream processing, exactly-once
  semantics
- **Apache Spark Structured Streaming:** Micro-batch streaming,
  familiar SQL API
- **dbt + Materialize:** SQL-based streaming transformations

**BI Role:** Powers real-time operational dashboards (e.g. live
fraud detection, live inventory levels), IoT sensor analytics,
and event-driven ML feature pipelines.

---

## 4.2 Comparative Summary

| Dimension          | OLTP Database  | Flat Files     | APIs           | Streaming      |
|--------------------|----------------|----------------|----------------|----------------|
| Structure          | Structured     | Semi/Structured| Semi-structured| Semi-structured|
| Latency            | Low–Medium     | Batch          | Low            | Real-time      |
| Volume             | Medium–High    | Low–High       | Low–Medium     | Very High      |
| Schema stability   | High           | Low            | Medium         | Low–Medium     |
| Access complexity  | Medium         | Low            | Medium         | High           |
| Extraction tool    | Airbyte, dbt   | Pandas, Spark  | Fivetran, dlt  | Kafka, Flink   |
| PII risk           | High           | Medium         | Medium–High    | High           |
| Cost to extract    | Low            | Very Low       | Medium         | High           |


#Conceptual Framework
5.1 An Integrated Model of BI Data Sources

The four source categories — transactional databases, flat files,
APIs, and streaming sources — are not mutually exclusive in practice.
A mature BI architecture ingests from all four simultaneously. The
conceptual framework below positions each source type relative to
two axes: **data velocity** (how fast data arrives) and **data
structure** (how well-defined its schema is).
High Structure
│
│   OLTP Databases        Flat Files (Parquet/CSV)
│   (MySQL, PostgreSQL)   (structured exports)
│
│─────────────────────────────────────────────── High Velocity
│
│   Streaming Sources     APIs (REST/GraphQL)
│   (Kafka, IoT, clicks)  (JSON responses)
│
Low Structure

## 5.2 Source Selection Decision Framework

When designing a BI pipeline, the choice of extraction method
should be guided by answering the following questions in order:

Does the source system expose a direct database connection?
├── YES → Use CDC (Debezium) for real-time or JDBC for batch
└── NO  → Proceed to question 2
Does the source provide an API?
├── YES → Use a managed connector (Fivetran, Airbyte) or
│         build a custom API extractor with OAuth 2.0
└── NO  → Proceed to question 3
Does the source produce file exports?
├── YES → Land files in object storage (S3/GCS), parse with
│         Spark or Pandas in the pipeline
└── NO  → Proceed to question 4
Does the source emit real-time events?
├── YES → Subscribe via Kafka/Pub-Sub and process with Flink
└── NO  → Manual data entry process — flag as a data quality
risk and escalate to the Data Steward


## 5.3 Governance Implications of Source Type

Each source type carries specific governance obligations that
must be addressed before data enters the warehouse:

| Source Type    | Key Governance Concern          | Mitigation                        |
|----------------|---------------------------------|-----------------------------------|
| OLTP Database  | PII in operational tables       | CDC with field-level masking      |
| Flat Files     | No access log, easy to copy     | Encrypt at rest, audit file access|
| APIs           | Data residency, third-party ToS | Review API ToS; check data region |
| Streaming      | High-velocity PII exposure      | Schema Registry + field encryption|

## 5.4 Positioning Within the BI Pipeline

Data sources are the entry point of the BI value chain. Their
characteristics determine every downstream design decision: