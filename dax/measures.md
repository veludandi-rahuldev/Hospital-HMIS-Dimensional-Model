# DAX Measures

This folder contains the standard DAX measures created for the Hospital HMIS dimensional model.

These measures cover commonly required KPIs and provide a consistent starting point for analysis.

Additional measures can be created by users based on specific business requirements.

---

## Standard Measures

### 1. Total Patients

```DAX
total_patients = COUNT(dim_patients[patient_id])
```

**Purpose:** Counts the total number of patient records in `dim_patients`.

---

### 2. Active Patients

```DAX
active_patients = DISTINCTCOUNT(snapshot_admission[patient_id])
```

**Purpose:** Counts distinct patients represented in the admission snapshot.

---

### 3. Current Drug Stock

```DAX
current_drug_stock = SUM(fact_drug_inventory[current_stock])
```

**Purpose:** Calculates the total current drug stock across inventory records.

---

### 4. Diagnostic Events

```DAX
diagnostic_events = COUNT(fact_patient_diagnostics[patient_diagnostic_id])
```

**Purpose:** Counts diagnostic events recorded in the diagnostic fact table.

---

### 5. Insurance Covered

```DAX
insurance_covered = SUM(fact_billing[insurance_covered_amount])
```

**Purpose:** Calculates the total amount covered by insurance.

---

### 6. Patient Payable

```DAX
patient_payable = SUM(fact_billing[patient_payable_amount])
```

**Purpose:** Calculates the total amount payable by patients.

---

### 7. Total Admissions

```DAX
total_admissions = COUNT(snapshot_admission[admission_id])
```

**Purpose:** Counts admission records in the admission snapshot.

---

### 8. Total Billing Amount

```DAX
total_billing_amount = SUM(fact_billing[total_amount])
```

**Purpose:** Calculates the total billing amount.

---

### 9. Total Bills

```DAX
total_bills = COUNT(fact_billing[bill_id])
```

**Purpose:** Counts billing records.

---

### 10. Total Prescriptions

```DAX
total_prescriptions = COUNT(fact_prescription[prescription_id])
```

**Purpose:** Counts prescription records.

---

## Usage Guidance

These measures are intended to provide **standard answers for common analytical requirements**.

They are not intended to cover every possible business question.

For additional analysis, users can create new DAX measures based on:

- Business requirement
- Fact table grain
- Required dimensions and filters
- Appropriate date relationship
- Required aggregation or calculation

The model should be understood before creating new measures. Users should first determine **which business process and fact table answer the question**, and then create the required calculation.

### Example

For an average Length of Stay (LOS), a user may create a measure based on the admission snapshot:

```DAX
average_los = AVERAGE(snapshot_admission[length_of_stay])
```

This is an example of a requirement-specific measure rather than one of the standard measures provided in the model.
