# Bank Loan Report Analysis

Welcome to the **Bank Loan Report Analysis** project! This repository contains SQL queries and a sample dataset to analyze loan performance for a financial institution. The project focuses on extracting key performance indicators (KPIs) such as total loan applications, funded amounts, amounts received, interest rates, debt-to-income (DTI) ratios, and loan status breakdowns, along with trends across months, states, terms, employment lengths, purposes, and home ownership types.

## Table of Contents
- [Project Overview](#project-overview)
- [Dataset](#dataset)
- [Prerequisites](#prerequisites)
- [Setup](#setup)
- [SQL Queries](#sql-queries)
- [Usage](#usage)


## Project Overview
This project uses SQL to analyze loan data from a bank, providing insights into loan issuance, repayment, and risk assessment. The analysis is divided into two main sections:
1. **Summary**: KPIs including total loan applications, funded amounts, amounts received, average interest rates, DTI, and breakdowns of good vs. bad loans.
2. **Overview**: Detailed breakdowns by month, state, loan term, employment length, loan purpose, and home ownership, with filters for deeper insights.

The dataset and SQL scripts are designed to help stakeholders evaluate loan performance, identify trends, and assess risk factors such as bad loan percentages.

## Dataset
The dataset (`bank_loan_data.csv`) contains loan application and repayment data with the following columns:
- `id`: Unique identifier for each loan application.
- `address_state`: State of the borrower's address (e.g., CA, NY, TX).
- `application_type`: Type of application (e.g., INDIVIDUAL).
- `emp_length`: Employment length (e.g., < 1 year, 10+ years).
- `emp_title`: Borrower's job title.
- `grade`: Loan grade (e.g., A, B, C).
- `home_ownership`: Home ownership status (e.g., RENT, MORTGAGE, OWN).
- `issue_date`: Date the loan was issued (e.g., 11-02-2021).
- `last_credit_pull_date`: Date of last credit pull.
- `last_payment_date`: Date of last payment.
- `loan_status`: Status of the loan (e.g., Fully Paid, Charged Off, Current).
- `next_payment_date`: Date of next scheduled payment.
- `member_id`: Unique member identifier.
- `purpose`: Purpose of the loan (e.g., car, wedding, debt consolidation).
- `sub_grade`: Sub-grade of the loan (e.g., A1, B5).
- `term`: Loan term (e.g., 36 months, 60 months).
- `verification_status`: Verification status (e.g., Verified, Not Verified).
- `annual_income`: Borrower's annual income.
- `dti`: Debt-to-income ratio.
- `installment`: Monthly installment amount.
- `int_rate`: Interest rate of the loan.
- `loan_amount`: Amount funded for the loan.
- `total_acc`: Total number of accounts.
- `total_payment`: Total amount received from the borrower.

A sample dataset is included in this repository, covering loan transactions from 2021.

## Prerequisites
To run this project, you’ll need:
- A SQL-compatible database (e.g., MySQL, PostgreSQL, SQLite).
- A tool to execute SQL queries (e.g., MySQL Workbench, DBeaver, or command-line interface).
- Basic knowledge of SQL for modifying or extending the queries.

## Setup
1. **Clone the Repository**:
   ```bash
   git clone https://github.com/your-username/bank-loan-report-analysis.git
   cd bank-loan-report-analysis
   ```

2. **Set Up the Database**:
   - Create a new database in your SQL environment:
     ```sql
     CREATE DATABASE bank_loan_db;
     USE bank_loan_db;
     ```
   - Create the table to hold the dataset:
     ```sql
     CREATE TABLE bank_loan_data (
         id INT,
         address_state VARCHAR(2),
         application_type VARCHAR(20),
         emp_length VARCHAR(20),
         emp_title VARCHAR(100),
         grade CHAR(1),
         home_ownership VARCHAR(20),
         issue_date VARCHAR(10),
         last_credit_pull_date VARCHAR(10),
         last_payment_date VARCHAR(10),
         loan_status VARCHAR(20),
         next_payment_date VARCHAR(10),
         member_id INT,
         purpose VARCHAR(50),
         sub_grade VARCHAR(5),
         term VARCHAR(20),
         verification_status VARCHAR(20),
         annual_income DECIMAL(15,2),
         dti DECIMAL(10,4),
         installment DECIMAL(10,2),
         int_rate DECIMAL(6,4),
         loan_amount INT,
         total_acc INT,
         total_payment INT
     );
     ```
   - Load the CSV data (example for MySQL):
     ```sql
     LOAD DATA INFILE 'path/to/bank_loan_data.csv'
     INTO TABLE bank_loan_data
     FIELDS TERMINATED BY ',' 
     ENCLOSED BY '"' 
     LINES TERMINATED BY '\n'
     IGNORE 1 ROWS;
     ```

3. **Run Initial Data Preparation Queries**:
   - Convert `issue_date` to a proper date format:
     ```sql
     UPDATE bank_loan_data
     SET issue_date = STR_TO_DATE(issue_date, '%d-%m-%Y');
     
     ALTER TABLE bank_loan_data
     MODIFY COLUMN issue_date DATE;
     ```

4. **Verify the Table Structure**:
   ```sql
   DESCRIBE bank_loan_data;
   ```

## SQL Queries
The `queries.sql` file contains a collection of SQL queries for analyzing the bank loan data. Key analyses include:
- **Summary KPIs**:
  - Total/MTD/PMTD loan applications, funded amounts, and amounts received.
  - Average interest rates and DTI (overall, MTD, PMTD).
  - Good loan vs. bad loan metrics (percentage, applications, funded amounts, received amounts).
- **Overview Breakdowns**:
  - Monthly trends (applications, funded amounts, received amounts).
  - State-wise performance.
  - Loan term analysis (36 vs. 60 months).
  - Employment length impact.
  - Loan purpose distribution.
  - Home ownership effects.
- **Filtered Analysis**: Example filters like `grade = 'A'` for dashboards.

To run a query, copy it from `queries.sql` and execute it in your SQL environment.

## Usage
1. Open your SQL client and connect to the `bank_loan_db` database.
2. Load and run the desired query from `queries.sql`. For example:
   - To calculate total funded amount by loan purpose:
     ```sql
     SELECT 
         purpose AS PURPOSE,
         COUNT(id) AS Total_Loan_Applications,
         SUM(loan_amount) AS Total_Funded_Amount,
         SUM(total_payment) AS Total_Amount_Received
     FROM bank_loan_data
     GROUP BY purpose
     ORDER BY purpose;
     ```
3. Export results to CSV or visualize them using tools like Tableau or Power BI for further analysis.

