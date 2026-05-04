## Definition
A Star Schema is a data warehouse design where a central
“Fact Table” is surrounded by Dimension Tables, forming
a star-like structure.

 ## Fact Table
Contains measurable, quantitative data (metrics) such as
sales amount, quantity sold, or revenue. Each row represents
a business event or transaction.

 ## Dimension Tables
Contain descriptive attributes used to filter, group, or label
facts. Common dimensions include:
- Date - year, quarter, month, day
- Product - category, name, SKU
- Customer - region, segment, age group
- Store - location, type, size

 ## Advantages
- Simple and intuitive structure for analysts
- Optimized for fast read queries (OLAP)
- Easy to understand and maintain.

