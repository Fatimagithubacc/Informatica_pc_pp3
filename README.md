# Informatica_pc_pp3

Production-grade ETL pipeline implementing SCD Type 1,2 and 3 dimension logic, audit validation, and Fact table loads.

## Repository Content
* `m_fact_emp_pp3.XML` - Fact table mapping with surrogate key lookups and target loads.
* `m_scd2_date_emp.XML` - SCD Type 2 dimension mapping for employee dates/history.
* `m_scd3_dept.XML` - SCD Type 3 dimension mapping for department tracking.
* `m_DIM_DEPT_AUDIT.XML` - Mapping for records reconcilation.
* `wf_Scd2_ppl3.XML` - Master workflow orchestrating audit sessions, decision checks, and fact execution.

## Workflow Execution Flow (`wf_Scd2_ppl3`)

1. ss_scd2_pP3: First dimension task executing SCD Type 2 logic to load/update employee history.
2. ss_dim_dept_Scd3: Second dimension task executing SCD Type 3 logic to update department track history.
3. ss_dept_audit: Audit validation session ensuring source/target reconciliation before loading facts.
4. reconciliation_test: Decision task checking if `$ss_dept_audit.Status = SUCCEEDED`.
   Path A (`TRUE`) $\rightarrow$ `ss_fact_emp_pp3`**: Loads the fact table once dimension and audit checks pass.
   Path B (`FALSE`) $\rightarrow$ `cmd_dept_audit`**: Triggers email/command alert notification if audit checks fail.
