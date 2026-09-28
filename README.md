# Hospital HMIS Dimensional Data Modeling Project

## One-Line Summary

Designed and implemented a Kimball dimensional data model for a Hospital Management Information System using Power BI, Power Query and DAX to support admissions, clinical, financial, insurance, pharmacy and staff analytics.

## Overview

This project transforms 19 raw HMIS source tables into a hybrid dimensional model designed around business processes and analytical grain.

The workflow covered data profiling, Power Query transformation, dimensional modeling, DAX measures, dashboard development, and independent validation against the original source data.

## Problem Statement

The source HMIS data was organized as normalized operational tables. The objective was to transform it into a reliable analytical model that enables cross-functional hospital reporting while avoiding incorrect relationships, double counting and ambiguous analytical paths.

## Dataset

- **Source:** https://www.kaggle.com/datasets/shalakagangurde/hospital-hmis-dataset-for-healthcare-analytics?resource=download
- 19 synthetic but realistic HMIS CSV tables
- Data period: 2020–2025
- Key areas: Patients, Admissions, Billing, Diagnostics, Prescriptions, Insurance, Drugs, Inventory, Employees and Staff Assignments
- 30,000 patients
- 45,000 admissions
- 73,109 prescriptions
- 63,269 diagnostic events

The original dataset is provided under the **MIT License**.

## Tools & Technologies

- Power BI
- Power Query
- DAX
- CSV
- Kimball Dimensional Modeling

## Project Highlights

### Data Preparation

- Profiled and classified 19 source tables by grain and business role.
- Organized source and analytical Power Query layers.
- Applied transformations required for dimensional modeling.

### Dimensional Modeling

- Designed a grain-first hybrid dimensional model.
- Created conformed dimensions.
- Used selective snowflaking where justified.
- Avoided fact-to-fact relationships.
- Created an accumulating admission snapshot.
- Implemented process-specific flag dimensions.
- Created standard DAX measures.

### Validation

- Independently calculated expected results from the original CSV files.
- Compared expected results with corresponding Power BI measures and visual outputs.
- Validated key KPIs, filters, relationships, grain and double-counting.
- Tested cross-fact isolation and admission snapshot calculations.

## Dashboard / Final Data Model

![Final Data Model](images/final_data_model.png)

The Power BI model supports analysis across admissions, clinical activity, billing, insurance, pharmacy and staff operations.

## How to Run / Explore

1. Clone or download the repository.
2. Open `data_model_file/hospital_hmis_dimensional_model.pbix` in Power BI Desktop.
3. Review the model relationships and standard DAX measures.
4. Refer to the project report for the complete modeling decisions and validation process.

### Repository Structure

```text
data/
└── raw/                  # Source HMIS CSV files

data_model_file/
└── hospital_hmis_dimensional_model.pbix

images/                   # Model and validation screenshots

workflow/
└── workflow_roadmap.png

report/
└── hmis_dimensional_modeling_report.pdf

dax/
└── measures.md

README.md
LICENSE
```

## Results & Conclusion

The final model provides a structured analytical layer for hospital management reporting while preserving business-process grain and minimizing double-counting and relationship ambiguity.

Key analytical areas include admissions, length of stay, billing, insurance coverage, diagnostics, prescriptions, inventory and staff assignments.

## Future Work

Future improvements would extend the model to support historical analysis and additional operational use cases that are not fully represented in the current source data.

- **Historical bed occupancy and utilization:** Add historical bed-status or occupancy records to analyze utilization over time.
- **Inventory movement history:** Capture stock receipts, issues and adjustments instead of only the current inventory state.
- **Staff scheduling history:** Add assignment dates and historical staffing changes to support workforce analysis over time.
- **Stronger insurance claim/policy linkage:** Establish a reliable relationship between patient policies, admissions and billing/claims.
- **Prescription dates:** Add prescription dates to enable time-based medication analysis.
- **Charge-level billing analysis:** Reintroduce billing-detail analysis if reliable reconciliation rules and business requirements are established.
- **Slowly Changing Dimensions:** Introduce appropriate SCD techniques when historical attribute tracking or multiple source systems require them.

## Limitations

- Dataset is synthetic.
- Inventory represents current state rather than historical movements.
- Staff assignments have no historical dates.
- Prescription records lack a prescription date.
- Billing-detail values were not reconciled with billing totals and were therefore excluded from the analytical model.
- Some source dates extend into January 2026 despite the stated 2020–2025 dataset period.

## Author & Contact

**Rahul Dev Valludandhi**

- GitHub: [veludandi-rahuldev](https://github.com/veludandi-rahuldev)
- LinkedIn: [rahuldev-v](https://www.linkedin.com/in/rahuldev-v)
- Email: veludandirahul@gmail.com

## License

This project is licensed under the **MIT License**. See the [LICENSE](LICENSE) file for details.

## Detailed Documentation

- **Project Report:** `report/hmis_dimensional_modeling_report.pdf`
- **Power BI Model:** `data_model_file/hospital_hmis_dimensional_model.pbix`
- **DAX Measures:** `dax/measures.md`
