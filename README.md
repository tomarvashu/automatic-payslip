# Automatic Payslip Workflow

An n8n-based payroll automation workflow that generates employee payslips as PDFs, emails them automatically, and updates delivery status in Google Sheets.

## Overview

This workflow is designed to automate the full monthly payslip process.

It:
- runs on a monthly schedule
- fetches employees marked as `Pending` from Google Sheets
- prepares employee payroll data
- generates a styled HTML payslip
- converts the HTML into a PDF using Gotenberg
- renames the PDF dynamically
- sends the payslip by email through Gmail
- updates the employee row in Google Sheets to `Sent`

The uploaded workflow file is named **`Payslip Complete Workflow Final.json`** and includes the full end-to-end flow, including schedule trigger, payload preparation, HTML generation, PDF conversion, email sending, and sheet status update.

---

## My Role

I initiated this workflow and defined the business logic, payroll data flow, and practical use case it was designed to support.

As my role later expanded into broader Operations & Technology leadership, my contribution shifted more toward workflow direction, testing, refinement, and execution oversight. Development and implementation were carried forward with support from my internal team, while I remained involved in shaping the system logic, usability, and practical execution flow.

---

## Workflow Features

- Monthly scheduled execution
- Google Sheets-based employee source
- Dynamic payroll field mapping
- HTML payslip generation
- PDF generation via Gotenberg
- Gmail-based delivery
- Automatic status update back to Google Sheets
- Batch processing for multiple employees

---

## Workflow Logic

### 1. Schedule Trigger
The workflow starts automatically on a monthly schedule using the node:

- **Monthly Schedule - 1st at 10 AM**

### 2. Fetch Pending Employees
It reads employee rows from Google Sheets and filters records where:

- `Status = Pending`

### 3. Loop Through Employees
Each employee is processed individually in batches.

### 4. Prepare Payload
The workflow standardizes payroll fields, calculates values, formats numbers, and creates derived fields such as:

- total earnings
- total deductions
- net salary
- net salary in words

This logic is handled in the **Prepare Payload** code node.

### 5. Build Payslip HTML
The workflow builds a complete HTML salary slip template with:

- company header
- employee details
- earnings table
- deductions table
- net payable section
- amount in words
- system-generated footer

This is done in the **Build HTML** node.

### 6. Convert HTML to PDF
The HTML is converted into a PDF file using:

- **Convert HTML to File**
- **Generate PDF (Gotenberg)**

### 7. Rename PDF
The generated PDF file is renamed dynamically using:

- employee ID
- salary month

### 8. Send Salary Slip
The PDF is emailed to each employee using Gmail with a dynamic subject and message. The **Send Salary Slip** node uses the `employee_email`, `salary_month`, and `employee_name` fields from the prepared payload.

### 9. Update Delivery Status
After sending the email, the workflow updates the corresponding row in Google Sheets:

- `Status = Sent`

### 10. Completion Message
Once all records are processed, the workflow returns a final success message.

---

## Tech Stack

- **n8n**
- **Google Sheets**
- **Gmail**
- **Gotenberg**
- **JavaScript (Code Nodes)**
- **HTML / CSS**

---

## Required Services

Before running this workflow, make sure the following are configured:

- n8n instance
- Google Sheets OAuth connection
- Gmail OAuth connection
- Gotenberg instance accessible from n8n
- Google Sheet with employee payroll data
- valid employee email addresses

---

## Expected Google Sheet Data

The workflow expects payroll-related employee data in Google Sheets.

Example fields used in the workflow include:

- `row_number`
- `employee_id`
- `employee_name`
- `employee_email`
- `salary_month`
- `generated_date`
- `Branch`
- `Designation`
- `Department`
- `DOJ`
- `Aadhar`
- `PAN`
- `UAN`
- `ESI No`
- `Account No`
- `Bank Name`
- `Basic Salary`
- `HRA`
- `Arrear`
- `PF`
- `ESI`
- `Advance`
- `Voluntary PF`
- `Professional Tax`
- `Total Deduction`
- `Net Salary`
- `Status`

The workflow also supports attendance- and leave-related fields such as:

- `PD`
- `WO`
- `Holiday`
- `CL`
- `PL`
- `CO`
- `RL`
- `WD`
- `LWP`
- `TDays`

These are mapped inside the **Prepare Payload** node.

---

## How to Use

### Import the Workflow
1. Open n8n
2. Import `Payslip Complete Workflow Final.json`
3. Reconnect credentials for:
   - Google Sheets
   - Gmail

### Configure Gotenberg
Make sure your Gotenberg URL is reachable from n8n.

### Prepare the Sheet
Make sure the Google Sheet:
- contains all required columns
- marks unsent employees with `Pending`

### Activate the Workflow
Once everything is configured:
1. test with one employee row first
2. verify PDF generation and email delivery
3. activate the workflow for scheduled monthly execution

---

## Output

For each employee, the workflow produces:

- one PDF payslip
- one email sent to the employee
- one Google Sheet row updated from `Pending` to `Sent`

---

## Notes

- This workflow is suitable for internal payroll automation use cases
- Before publishing or sharing publicly, remove:
  - confidential sheet IDs
  - internal email addresses
  - organization-specific identifiers
  - private infrastructure URLs if needed

---

## File

- `Payslip Complete Workflow Final.json`
