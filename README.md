# Junior_Software_Ex
Detects patients with >3 claims in the past year — adaptable for clearinghouse claim validation, secure transfer (MOVEit/Axway), and cloud (.NET/Python/AWS) pipelines
# High-Frequency Claims Detection – SQL Demo

## Overview
This project demonstrates how to identify patients with more than three claims in the past 12 months.  
In a healthcare clearinghouse context, this logic can be adapted to flag high-volume claim submitters, detect unusual patterns, or trigger compliance reviews before claims are routed to payers.  
While this demo runs in SQLite, the logic is portable to enterprise environments using AWS (RDS/Redshift), Python ETL scripts, .NET applications, and secure transfer solutions like Axway or MOVEit — all while maintaining HIPAA compliance.

## Tech Stack Awareness
- **SQLite** for the demo (portable to PostgreSQL, SQL Server, AWS RDS)
- **Python** for automation and ETL integration
- **.NET** for application-layer integration
- **AWS** for cloud-based deployment and scaling
- **Axway / MOVEit** for secure, audited file transfers

## How It Works
1. **Create Table** – Defines the `claims` table with claim ID, patient ID, provider ID, claim date, and claim amount.
2. **Insert Sample Data** – Populates the table with representative claims over a 12+ month period.
3. **Run Query** – Filters for claims in the last year, groups by patient, and returns only those with more than three claims.

## Full Demo Script
```sql
-- Reset table
DROP TABLE IF EXISTS claims;

-- Create claims table
CREATE TABLE claims (
  claim_id INTEGER,
  patient_id INTEGER,
  provider_id INTEGER,
  claim_date DATE,
  claim_amount NUMERIC
);

-- Insert sample claims
INSERT INTO claims VALUES
(1,101,9001,'2024-09-01',120.00),
(2,101,9001,'2024-10-15',200.00),
(3,101,9001,'2025-01-20',180.00),
(4,101,9001,'2025-03-05',140.00),
(5,102,9002,'2025-02-10',90.00),
(6,103,9003,'2025-04-22',300.00),
(7,101,9001,'2025-05-10',160.00),
(8,104,9004,'2025-06-15',110.00);

-- Query: patients with >3 claims in past year
SELECT patient_id
FROM claims
WHERE claim_date >= date('now', '-1 year')
GROUP BY patient_id
HAVING COUNT(*) > 3;
