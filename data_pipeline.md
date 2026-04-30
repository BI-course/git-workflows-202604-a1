# Research on ETL, ELT, and EtLT 
Differentiating between ETL, ELT, and EtLT in the context of compliance with the legal requirements in an industry.

## Introduction
The ETL (Extract, Transform, Load) process is a process of populating a data warehouse from its source systems in a form that is accurate, consistent, and analytically useful. 
This has given rise to the ELT paradigm. 

## Literature review
1. ETL:
- It is a traditional approach where data is pulled for sources, converted in a separate processing engine and then loaded into a data warehouse. 
- It is best used for legacy systems, highly structured data, or systems that require strict compliance before storage. 

2. ELT:
- It is the modern standard for cloud-based data architectures. It was made popular by systems like BigQuery. The raw data is immediately loaded into the warehouse. 
- It is best used for big data, cloud data warehouses, and data lakes. 

3. EtLT:
- EtLT combines both paradigms. It cleans and prepares data lightly before loading and performs heavy, business specific transformations after loading. 
- It is best used for complex ETL pipelines that require PII protection, IoT data, or third party APIs. 

# Conclusion
- ETL is the best for strict security and legacy systems
- ELT is the best for mordern, fast, moving, and scalable data teams.
- EtLT is best for balancing data quality with analytical flexibility