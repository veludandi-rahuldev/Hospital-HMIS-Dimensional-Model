# Hospital HMIS — Dimensional Data Modeling Project

> A Power BI dimensional modeling project that transforms a normalized Hospital Management Information System (HMIS) dataset into a scalable analytical model using Kimball dimensional modeling principles.

## 1. Project Objective

The objective of this project was to design a reliable analytical data model for a hospital by transforming 19 source HMIS tables into a business-oriented dimensional model in Power BI.

The project focuses on:
- Identifying business processes and analytical requirements
- Defining the correct grain of each fact table
- Designing dimensions and fact tables using Kimball principles
- Preventing fan-out and double-counting
- Creating conformed dimensions and appropriate date relationships
- Designing reusable DAX measures
- Validating the model against the original source data
- Providing clear rules for using the model for future analysis

## 2. Business Problem

A hospital generates data across multiple operational areas such as patient admissions, billing and payments, diagnostics, prescriptions, insurance, drug inventory, staff assignments, and patient journey.

The source data is normalized across multiple related tables. Directly connecting and analyzing these tables can lead to ambiguous relationships, incorrect aggregation, double-counting, and difficult-to-maintain reports.

The goal was therefore to create a dimensional model that provides a consistent analytical foundation for hospital management and different functional teams.

## 3. Project Overview

The project followed a business-process-first and grain-first approach.

### Overall workflow
1. Source data review
2. Data profiling
3. Power Query preparation
4. Kimball dimensional modeling
5. DAX measure development
6. Dashboard development
7. Model testing and validation
8. Data Model Usage Guide

The final model uses a **hybrid dimensional architecture**: a star schema is preferred where appropriate, with selective dimension-side flattening/snowflake structures where justified.

## 4. Dataset

The project uses a synthetic but realistic HMIS dataset containing **19 source tables** covering hospital operations from approximately 2020–2025.

| Source Table | Approx. Rows | Initial Role / Grain |
|---|---:|---|
| department | 11 | Dimension — department |
| ward | 27 | Dimension — ward |
| bed | 415 | Dimension — bed |
| disease | 20 | Dimension — disease |
| diagnostic_test | 9 | Dimension — diagnostic test |
| drug_manufacturer | 300 | Reference / dimension |
| insurance_provider | 50 | Dimension — provider |
| patient | 30,000 | Dimension — patient |
| admission | 45,000 | Encounter / admission |
| billing | 45,000 | Fact — bill |
| billing_detail | 112,402 | Fact candidate — billing detail |
| patient_diagnostic | 63,269 | Fact — diagnostic event |
| drug | 250 | Dimension — drug |
| drug_inventory | 250 | Snapshot fact |
| prescription | 73,109 | Fact — prescription |
| patient_insurance | 21,617 | Fact — insurance policy |
| employee | 500 | Dimension — employee |
| doctor | 98 | Dimension — doctor |
| staff_assignment | 207 | Fact — assignment |

## 5. Business Requirements

### Operations
- Admission volume and trends
- Admission type and status
- Department, ward and bed analysis
- Length of stay
- Patient journey

### Clinical
- Disease patterns
- Diagnostic activity
- Diagnostic results
- Doctor activity
- Prescription activity

### Finance
- Billing amount
- Insurance-covered amount
- Patient-payable amount
- Payment status
- Payment mode
- Billing trends

### Insurance
- Insurance providers
- Insured patients
- Coverage percentages
- Policy records

### Pharmacy / Inventory
- Drugs and manufacturers
- Current stock
- Reorder levels
- Low-inventory drugs
- Inventory status

### HR / Staff
- Employees
- Doctors
- Ward assignments
- Shift distribution

## 6. Modeling Approach

The model follows key Kimball dimensional modeling principles:

- **Grain-first design**
- **Business-process-first modeling**
- Facts and dimensions separated by analytical purpose
- Conformed dimensions where appropriate
- No fact-to-fact relationships
- Star schema as the default
- Selective dimension-side flattening where justified
- Appropriate handling of transaction and snapshot facts
- Controlled use of inactive relationships
- Prevention of fan-out and double-counting

### Source → Analytical transformation

- `drug + drug_manufacturer` → `dim_drugs`
- `ward + department` → `dim_wards`
- `bed + ward + department` → `dim_beds`
- `employee + department` → `dim_employees`
- `diagnostic_test + department` → `dim_diagnostic_tests`

The `admission` source table was modeled as both:
- `dim_admission` — admission-level analytical context
- `snapshot_admission` — accumulating snapshot representing the admission journey

Child processes such as diagnostics and prescriptions were aggregated to admission grain before being incorporated into the admission snapshot to prevent fan-out.

## 7. Key Modeling Decisions

### Admission modeling
`dim_admission` provides admission-level context for active patients, admission type, disease, and admission-related filtering.

`snapshot_admission` represents the admission lifecycle at one row per admission and contains journey-level information and selected admission-level summaries.

### Doctor and Employee
`doctor` was kept separate from `employee`. Doctor is a specialized subset of employees with doctor-specific attributes. Merging the two would unnecessarily widen the employee dimension and introduce sparse doctor-specific attributes.

The doctor → employee relationship is maintained separately and is inactive.

### Billing
`fact_billing` is the official financial fact at bill grain.

`billing_detail` was retained for traceability but excluded from the final analytical semantic model because it has a different grain, its values did not directly reconcile with billing totals, and there was no current charge-level analytical requirement.

### Flag / Junk Dimensions
Small process-specific flag dimensions were created for low-cardinality attributes:
- `dim_billing_flag`
- `dim_drug_inventory_flag`
- `dim_patient_diagnostics_flag`
- `dim_prescription_flag`
- `dim_staff_assignment_flag`

These keep the main fact tables focused on their business-process grain and measures.

## 8. Final Data Model

### Dimensions
- `dim_patients`
- `dim_admission`
- `dim_beds`
- `dim_wards`
- `dim_diseases`
- `dim_doctors`
- `dim_employees`
- `dim_diagnostics_tests`
- `dim_drugs`
- `dim_insurance_provider`
- `dim_date`

### Facts / Snapshots
- `fact_billing`
- `fact_patient_diagnostics`
- `fact_prescription`
- `fact_patient_insurance`
- `fact_drug_inventory`
- `fact_staff_assignment`
- `snapshot_admission`

![Final Data Model](images/final_data_model.png)

## 9. Power BI Dashboard

The Power BI implementation provides analytical views across hospital operations, finance, clinical activity, pharmacy/inventory, insurance and staff-related processes.

Standard DAX measures include:
- Total Patients
- Active Patients
- Total Admissions
- Total Bills
- Total Billing Amount
- Insurance Covered
- Patient Payable
- Diagnostic Events
- Total Prescriptions
- Current Drug Stock

The `.pbix` file is available under `data_model_file/`.

## 10. Validation & Testing

The model was validated by independently calculating expected results from the original source CSV files and comparing them with the corresponding Power BI measures and visual outputs.

Validation covered:
- Overall KPI values
- Admission-type filtering
- Payment-mode filtering
- Department and ward filtering
- Patient-level filtering
- Disease, doctor, diagnostic-test and drug filtering
- Billing reconciliation
- Fact-table grain
- Double-counting / fan-out checks
- Relationship behavior
- Cross-fact isolation
- Admission snapshot calculations

### Selected validation results

| Metric | Expected Result |
|---|---:|
| Total Patients | 30,000 |
| Total Admissions | 45,000 |
| Total Bills | 45,000 |
| Total Billing Amount | ₹1,684,246,109 |
| Insurance Covered | ₹976,880,867 |
| Patient Payable | ₹707,365,242 |
| Diagnostic Events | 63,269 |
| Prescriptions | 73,109 |
| Insurance Policy Records | 21,617 |
| Current Drug Stock | 130,878 |
| Low Inventory Drugs | 44 |
| Employees | 500 |
| Doctors | 98 |

### Billing reconciliation

`Insurance Covered + Patient Payable = Total Billing Amount`

`₹976,880,867 + ₹707,365,242 = ₹1,684,246,109`

## 11. Data Model Usage Guide

**Business question → Business process → Required grain → Fact / Dimension → Standard measure → Date context → Filters → Relationship path → DAX if required → Validate → Visualize**

| Business Question | Recommended Analytical Path |
|---|---|
| Total number of admissions | `snapshot_admission` |
| Active patients | `dim_admission` |
| Admissions by type | `dim_admission` |
| Admissions by disease | `dim_disease → dim_admission` |
| Admissions by department | `dim_beds → dim_admission` |
| Admissions by year | `dim_date → snapshot_admission` |

Not every KPI should respond to every slicer. Filter behavior should follow the intended business relationship rather than modifying the model merely to make every visual respond to every filter.

## 12. Date Dimension

A conformed `dim_date` was implemented for relevant analytical processes.

Active date relationships include:
- Billing → `bill_date`
- Diagnostics → `test_date`
- Drug inventory → `last_restock_date`
- Admission snapshot → `admission_date`

An inactive relationship is maintained for:
- Admission snapshot → `discharge_date`

The model does not create a prescription date relationship because the source prescription table does not contain a prescription date.

## 13. Repository Structure

```text
Hospital-HMIS-Dimensional-Model/
│
├── README.md
├── .gitignore
│
├── data/
│   └── raw/
│       └── 19 source CSV files
│
├── data_model_file/
│   └── hospital_hmis_dimensional_model.pbix
│
├── images/
│   ├── source_data_model.png
│   ├── final_data_model.png
│   ├── power_query_structure.png
│   └── validation screenshots
│
├── workflow/
│   └── workflow_roadmap.png
│
├── report/
│   └── hmis_dimensional_modeling_report.pdf
│
└── dax/
    └── measures.md
```

## 14. Limitations & Future Improvements

### Limitations
- No historical bed occupancy/utilization data
- Drug inventory represents current state rather than movement history
- Staff assignments do not contain assignment dates/history
- Prescription records do not contain a prescription date
- Limited linkage between insurance policies and individual billing/claims
- `billing_detail` is excluded from the analytical model due to reconciliation and requirement considerations
- Source dates extend into January 2026 despite the stated 2020–2025 dataset period
- The dataset is synthetic

### Future improvements
- Historical bed occupancy fact
- Inventory movement fact
- Dated staff scheduling and assignment history
- Stronger insurance claim/policy linkage
- Prescription date
- Charge-level billing analysis once business rules are established
- Slowly Changing Dimensions if historical or multi-source requirements are introduced
- Additional data-quality and audit mechanisms

## 15. Tools & Technologies

- **Power BI**
- **Power Query**
- **DAX**
- **Kimball Dimensional Modeling**
- **GitHub**
- **CSV / Relational Source Data**

## 16. Project Documentation

For a detailed explanation of the modeling journey, design decisions, challenges, validation and usage rules, see:

`report/hmis_dimensional_modeling_report.pdf`

## 17. Author

**Rahul Dev Valludandhi**

Civil Engineering Graduate | Aspiring Data Analyst

Interested in:
- Data Analytics
- Business Intelligence
- Dimensional Data Modeling
- Business Problem Solving
- Power BI

## Disclaimer

This project uses a synthetic HMIS dataset created for analytical and portfolio purposes. It does not contain real patient or hospital data.
