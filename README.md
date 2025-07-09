# 🏒Sports Club Project (SQL Edition)

This project uses **SQL** to analyse and manage member payment data for a local hockey club. The goal is to help the club:

- Track unpaid invoices  
- Spot members with repeated payment problems  
- Understand payment trends over time  
- Support better financial decision-making

These insights are essential for club administrators to improve member communication, budgeting, and overall operations.

---

## ✅ What This Project Does

Using structured SQL queries, the project extracts meaningful insights from two key data sources:

- **Members Table** – contains member info such as name, age, and club section (e.g., Junior or Senior)
- **Invoices Table** – holds data on payments, due dates, statuses (paid, unpaid, or late), and amounts

---

## 🔍 Key Reports Generated

### 1. 🧾 Unpaid Invoices by Age Group and Club Section

Breaks down all unpaid invoices into age brackets and groups them by club section. Helps identify which groups have the highest unpaid amounts.

```sql
SELECT
  CASE
    WHEN TIMESTAMPDIFF(YEAR, m.dob, CURDATE()) < 12 THEN 'Under 12'
    WHEN TIMESTAMPDIFF(YEAR, m.dob, CURDATE()) BETWEEN 12 AND 17 THEN '12-17'
    WHEN TIMESTAMPDIFF(YEAR, m.dob, CURDATE()) BETWEEN 18 AND 30 THEN '18-30'
    ELSE '30+'
  END AS age_group,
  m.club_section,
  COUNT(i.invoice_id) AS unpaid_invoices,
  SUM(i.amount_due - i.amount_paid) AS total_unpaid
FROM Invoices i
JOIN Members m ON i.member_id = m.member_id
WHERE i.amount_due > i.amount_paid
GROUP BY age_group, m.club_section
ORDER BY age_group, m.club_section;


