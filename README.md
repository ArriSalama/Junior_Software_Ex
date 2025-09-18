# Junior_Software_Ex
Detects patients with >3 claims in the past year — adaptable for clearinghouse claim validation, secure transfer (MOVEit/Axway), and cloud (.NET/Python/AWS) pipelines
# High-Frequency Claims Detection – SQL Demo

## Overview
This is a simple SQL project I built to show how you can spot patients who’ve had more than three claims in the past year. In a real healthcare clearinghouse, this kind of check could help flag high‑volume claim submitters, catch unusual patterns, or send certain claims to compliance for review before they go to the payer.

I’m running the demo in SQLite because it’s lightweight and easy to share, but the same logic could be used in bigger, enterprise setups — like AWS (RDS or Redshift), inside Python ETL scripts, in .NET applications, or as part of a secure file transfer process with tools like Axway or MOVEit. And of course, everything would be handled in a HIPAA‑compliant way.

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
