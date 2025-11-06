# Payroll Management System - Multi-Dimensional Data Modeling

**Converting OLTP to OLAP for Advanced Payroll Analytics**

A data warehousing project developed as part of the Data Warehousing & Integration course (IE 6750, Fall 2024) at Northeastern University. It transforms transactional payroll data (OLTP) into an analytical data warehouse (OLAP) using Talend ETL pipeline and dimensional modeling techniques.

[![PostgreSQL](https://img.shields.io/badge/PostgreSQL-12%2B-blue.svg)](https://www.postgresql.org/)
[![Talend](https://img.shields.io/badge/Talend-7.3%2B-orange.svg)](https://www.talend.com/)

##  Overview

This project demonstrates the end-to-end process of **converting an OLTP (Online Transaction Processing) system to an OLAP (Online Analytical Processing) data warehouse** for payroll management. The transformation enables complex analytical queries and business intelligence reporting while maintaining operational data integrity.

**Key Achievement:** **40% improvement in query performance** by converting normalized OLTP schema to optimized Starflake dimensional model with Talend ETL pipeline.

##  Problem Statement

Organizations need to analyze payroll data for strategic decision-making, but transactional OLTP systems are optimized for operations, not analytics. Running complex analytical queries on OLTP systems leads to:

- **Poor Performance**: Multiple joins across normalized tables
- **Operational Interference**: Analytical queries impact transaction processing  
- **Limited Historical Analysis**: Difficulty tracking trends over time
- **No Multi-Dimensional Analysis**: Cannot slice and dice data efficiently

**Solution:** Build a separate OLAP data warehouse optimized for analytical workloads, populated through an ETL pipeline that transforms OLTP data into a dimensional model.

##  Architecture

### Three-Stage Data Pipeline
```
┌─────────────┐         ┌──────────────┐         ┌─────────────┐
│             │         │              │         │             │
│  OLTP       │ Extract │  Talend ETL  │  Load   │  OLAP       │
│  (Source)   ├────────►│  Transform   ├────────►│  (Target)   │
│             │         │              │         │             │
│ PostgreSQL  │         │  Pipeline    │         │ PostgreSQL  │
│ Normalized  │         │              │         │ Starflake   │
└─────────────┘         └──────────────┘         └─────────────┘
```

### OLTP vs OLAP Comparison

| Aspect | OLTP (Source) | OLAP (Target) |
|--------|---------------|---------------|
| **Purpose** | Day-to-day payroll operations | Analysis & reporting |
| **Schema** | Normalized (3NF) - 15+ tables | Denormalized (Starflake) - 1 Fact + 7 Dimensions |
| **Queries** | Simple INSERT/UPDATE/DELETE | Complex SELECT with aggregations |
| **Users** | Many operational users | Few analytical users |
| **Data Scope** | Current transactional data | Historical & aggregated data |
| **Performance** | Optimized for writes | Optimized for reads |
| **Joins** | Many small tables with 5-10 joins | Pre-joined fact table with dimensions |

##  OLTP to OLAP Transformation

### OLTP Schema (Before)
- **15 normalized tables** in 3NF
- Employee types split across 3 tables (FullTime, PartTime, Contractor)
- Payroll data scattered across 5+ tables
- Requires 5-10 joins for simple analysis

### ETL Process (Talend)
1. **Extract** from OLTP PostgreSQL
2. **Transform**: Consolidate, calculate, build hierarchies
3. **Load** into OLAP with Starflake schema

### OLAP Schema (After)
- **1 Fact Table**: Payroll_Fact
- **7 Dimension Tables**: Employee, Department, Division, Designation, Date, Overtime, Deductions
- **Hierarchies**: Organizational, Temporal, Geographic, Job Level

##  Technologies Used

| Technology | Version | Purpose |
|------------|---------|---------|
| PostgreSQL | 12+ | OLTP source and OLAP target database |
| Talend Open Studio | 7.3+ | ETL pipeline development |
| SQL | - | Schema definition and queries |


##  Data Model

### Conceptual Data Warehouse Model
<img width="499" height="510" alt="image" src="https://github.com/user-attachments/assets/9c75cd35-4b5f-4105-b3ec-b918a0557d61" />

### Relational OLAP Model
<img width="528" height="534" alt="image" src="https://github.com/user-attachments/assets/94a1c3fe-e860-41ec-9eaa-e9eef62e78a5" />


### Key Measures
- Gross_Salary
- Avg_Base_Pay (calculated)
- Overtime_Pay
- Deductions
- Number_of_Employees

### Hierarchies
- **Organizational**: Employee → Department → Division
- **Temporal**: Date → Month → Quarter → Year  
- **Geographic**: City → State → Country
- **Job**: Employee → Designation → Pay Grade

##  OLAP Operations Supported

1. **Roll-Up** - Aggregate to higher levels (Employee → Department → Division)
2. **Drill-Down** - Decompose to lower levels (Year → Quarter → Month)
3. **Slice** - Filter by single dimension
4. **Dice** - Filter by multiple dimensions
5. **Pivot** - Rotate data perspectives

##  Performance Results

### Query Performance Comparison

**Test Query:** "Total Payroll by Department for 2024"

| Metric | OLTP | OLAP | Improvement |
|--------|------|------|-------------|
| Execution Time | 850ms | 510ms | **40% faster** |
| Number of Joins | 5 | 2 | 60% reduction |
| Rows Scanned | 15,000+ | 100-500 | 97% reduction |

**Why OLAP is Faster:**
- Denormalized structure with pre-joined data
- Optimized indexes on dimension keys
- Pre-calculated measures during ETL
- Fewer table joins required


**Course:** IE 6750 - Data Warehousing & Integration  
**Institution:** Northeastern University, College of Engineering  
**Term:** Fall 2024

##  License

This project is part of academic coursework at Northeastern University.

---

**Skills Demonstrated:** Data Warehousing • Dimensional Modeling • ETL Development • SQL Optimization • Talend • PostgreSQL • Data Architecture
