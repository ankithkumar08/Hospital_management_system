
# 🏥 Hospital Management System – SQL Project

## 📋 Overview

This **Hospital Management System** project simulates a real-world healthcare database designed using SQL. It integrates key hospital operations such as **patient registration**, **doctor management**, **appointments**, **treatments**, and **billing**. The database is structured to enable meaningful data analysis and reporting that can help administrators optimize hospital performance and improve patient care.

---

## 🗂️ Database Schema

The database, `hospitals_db`, consists of the following tables:

### 1. patients
Stores details of all registered patients.

| Column Name       | Data Type   | Description                |
|-------------------|-------------|----------------------------|
| patient_id        | VARCHAR(10) | Primary Key                |
| first_name        | VARCHAR(50) | First name of patient      |
| last_name         | VARCHAR(50) | Last name of patient       |
| gender            | VARCHAR(10) | Gender                     |
| date_of_birth     | DATE        | Date of Birth              |
| contact_number    | VARCHAR(15) | Contact number             |
| address           | TEXT        | Address                    |
| registration_date | DATE        | Registration date          |
| insurance_provider| VARCHAR(100)| Insurance company          |
| insurance_number  | VARCHAR(50) | Insurance ID               |
| email             | VARCHAR(100)| Email address              |

### 2. doctors
Stores information about doctors.

| Column Name       | Data Type   | Description              |
|-------------------|-------------|--------------------------|
| doctor_id         | VARCHAR(10) | Primary Key              |
| first_name        | VARCHAR(50) | First name               |
| last_name         | VARCHAR(50) | Last name                |
| specialization    | VARCHAR(100)| Area of expertise        |
| phone_number      | VARCHAR(15) | Contact number           |
| years_experience  | INT         | Years of experience      |
| hospital_branch   | VARCHAR(100)| Branch location          |
| email             | VARCHAR(100)| Email address            |

### 3. appointments
Links patients with doctors and stores appointment details.

| Column Name        | Data Type   | Description                    |
|--------------------|-------------|--------------------------------|
| appointment_id     | VARCHAR(10) | Primary Key                    |
| patient_id         | VARCHAR(10) | Foreign Key → patients         |
| doctor_id          | VARCHAR(10) | Foreign Key → doctors          |
| appointment_date   | DATE        | Date of appointment            |
| appointment_time   | TIME        | Time of appointment            |
| reason_for_visit   | TEXT        | Reason for consultation        |
| status             | VARCHAR(50) | Status (Scheduled, Cancelled)  |

### 4. treatments
Stores treatment details given to patients.

| Column Name     | Data Type   | Description                 |
|-----------------|-------------|-----------------------------|
| treatment_id    | VARCHAR(10) | Primary Key                 |
| appointment_id  | VARCHAR(10) | Foreign Key → appointments  |
| treatment_type  | VARCHAR(100)| Type of treatment           |
| description     | TEXT        | Treatment description       |
| cost            | DECIMAL     | Cost of the treatment       |
| treatment_date  | DATE        | Treatment date              |

### 5. billing
Stores billing and payment details.

| Column Name     | Data Type   | Description                     |
|-----------------|-------------|----------------------------------|
| bill_id         | VARCHAR(10) | Primary Key                     |
| patient_id      | VARCHAR(10) | Foreign Key → patients          |
| treatment_id    | VARCHAR(10) | Foreign Key → treatments        |
| bill_date       | DATE        | Billing date                    |
| amount          | DECIMAL     | Amount billed                   |
| payment_method  | VARCHAR(50) | Payment method (Cash/Card/etc.)|
| payment_status  | VARCHAR(20) | Status (Paid/Pending/Failed)   |

---

## 📊 SQL Queries and Insights

### 🔍 Patient Insights

- Number of patients registered each month.
- Gender distribution of patients.
- Patient with the highest number of appointments.
- Duplicate patient records by name and DOB.

### 🧑‍⚕️ Doctor & Specialization Insights

- Doctor with most appointments.
- Average patient age treated by each doctor.
- Most popular specialization by visit count.
- Most experienced branch.

### 🕒 Appointment Analysis

- Count of 'No-show' or 'Cancelled' appointments.
- Most active days for appointments.
- Patients visiting multiple doctors.
- Patients visiting same doctor multiple times.

### 💉 Treatment Analysis

- Monthly treatment counts.
- Most common treatment.
- Highest average cost treatment.
- Average treatment cost by doctor.

### 💳 Billing & Payments

- Total billed vs. paid amount.
- Patients with pending or failed payments.
- Revenue per insurance provider.
- Most used payment method.

### 💰 Financial Reports

- Top 5 doctors by billed amount.
- Revenue by treatment type.
- Patient who spent the most.

-- 1. Patient registrations per month
SELECT MONTH(registration_date) AS month, COUNT(*) AS count 
FROM patients 
GROUP BY month;

-- 2. Gender distribution of patients
SELECT gender, COUNT(*) AS total 
FROM patients 
GROUP BY gender;

-- 3. Patient with most appointments
SELECT patient_id, COUNT(*) AS total 
FROM appointments 
GROUP BY patient_id 
ORDER BY total DESC 
LIMIT 1;

-- 4. Doctor with most appointments
SELECT doctor_id, COUNT(*) AS total 
FROM appointments 
GROUP BY doctor_id 
ORDER BY total DESC 
LIMIT 1;

-- 5. Average patient age per doctor
SELECT a.doctor_id, ROUND(AVG(TIMESTAMPDIFF(YEAR, p.date_of_birth, CURDATE()))) AS avg_age
FROM appointments a
JOIN patients p ON a.patient_id = p.patient_id
GROUP BY a.doctor_id;

-- 6. Appointments marked as No-show or Cancelled
SELECT status, COUNT(*) 
FROM appointments 
WHERE status IN ('No-show', 'Cancelled') 
GROUP BY status;

-- 7. Most popular specialization
SELECT specialization, COUNT(*) AS visits
FROM doctors d
JOIN appointments a ON d.doctor_id = a.doctor_id
GROUP BY specialization 
ORDER BY visits DESC 
LIMIT 1;

-- 8. Average appointments per doctor per month
SELECT doctor_id, AVG(monthly_appointments) AS average
FROM (
  SELECT doctor_id, MONTH(appointment_date) AS month, COUNT(*) AS monthly_appointments
  FROM appointments
  GROUP BY doctor_id, month
) AS sub
GROUP BY doctor_id;

-- 9. Hospital branch with most experienced doctors
SELECT hospital_branch, SUM(years_experience) AS total_exp
FROM doctors
GROUP BY hospital_branch
ORDER BY total_exp DESC
LIMIT 1;

-- 10. Top 5 doctors by billed treatment amount
SELECT a.doctor_id, SUM(t.cost) AS total_billed
FROM appointments a
JOIN treatments t ON a.appointment_id = t.appointment_id
GROUP BY a.doctor_id
ORDER BY total_billed DESC 
LIMIT 5;

-- 11. Revenue by treatment type
SELECT treatment_type, SUM(cost) AS total_revenue 
FROM treatments 
GROUP BY treatment_type;

-- 12. Treatment with highest average cost
SELECT treatment_type, AVG(cost) AS avg_cost 
FROM treatments 
GROUP BY treatment_type 
ORDER BY avg_cost DESC 
LIMIT 1;

-- 13. Treatments by month
SELECT MONTH(treatment_date) AS month, COUNT(*) 
FROM treatments 
GROUP BY month 
ORDER BY month;

-- 14. Most common treatment type
SELECT treatment_type, COUNT(*) AS total 
FROM treatments 
GROUP BY treatment_type 
ORDER BY total DESC 
LIMIT 1;

-- 15. Average treatment cost per doctor
SELECT a.doctor_id, AVG(t.cost) AS avg_cost
FROM appointments a
JOIN treatments t ON a.appointment_id = t.appointment_id
GROUP BY a.doctor_id;

-- 16. Total billed vs amount paid
SELECT SUM(t.cost) AS total_cost, SUM(b.amount) AS total_paid,
SUM(t.cost) - SUM(b.amount) AS outstanding
FROM billing b
JOIN treatments t ON b.treatment_id = t.treatment_id;

-- 17. Most used payment method
SELECT payment_method, COUNT(*) AS count 
FROM billing 
GROUP BY payment_method 
ORDER BY count DESC 
LIMIT 1;

-- 18. Patients with pending or failed payments
SELECT patient_id, payment_status 
FROM billing 
WHERE payment_status IN ('Pending', 'Failed');

-- 19. Revenue by insurance provider
SELECT insurance_provider, SUM(b.amount) AS revenue
FROM billing b
JOIN patients p ON b.patient_id = p.patient_id
GROUP BY insurance_provider;

-- 20. Treatment types with failed payments
SELECT t.treatment_type, COUNT(*) AS failed_count
FROM billing b
JOIN treatments t ON b.treatment_id = t.treatment_id
WHERE b.payment_status = 'Failed'
GROUP BY t.treatment_type
ORDER BY failed_count DESC;

-- 21. Patient who spent the most
SELECT patient_id, SUM(amount) AS total_spent 
FROM billing 
GROUP BY patient_id 
ORDER BY total_spent DESC 
LIMIT 1;

-- 22. Patients with partial payments
SELECT b.patient_id, SUM(b.amount) AS amount_paid, (SUM(t.cost) - SUM(b.amount)) AS balance
FROM billing b
JOIN treatments t ON b.treatment_id = t.treatment_id
GROUP BY b.patient_id
HAVING balance > 0;

-- 23. Insurance provider with most patients
SELECT insurance_provider, COUNT(*) AS count 
FROM patients 
GROUP BY insurance_provider 
ORDER BY count DESC 
LIMIT 1;

-- 24. Busiest appointment days
SELECT DAYNAME(appointment_date) AS weekday, COUNT(*) AS total
FROM appointments
GROUP BY weekday
ORDER BY total DESC;

-- 25. Patients with multiple appointments with the same doctor
SELECT patient_id, doctor_id, COUNT(*) AS total
FROM appointments
GROUP BY patient_id, doctor_id
HAVING total > 1
ORDER BY total DESC;

---

## ✅ Usage

You can run all the `CREATE`, `INSERT`, and `SELECT` queries in any MySQL-compatible SQL client (like MySQL Workbench or phpMyAdmin). Modify or expand the queries for additional insights as needed.

---

## 👨‍💻 Author

**Ankithkumar Chillapalli**  
B.Tech in CSE (AI & ML) | Aspiring Data Analyst | Passionate about SQL and Data Analytics



## Connect with Me 🤝
Find out more about my journey and connect with me on [LinkedIn](https://www.linkedin.com/in/ankithkumar08-data-analyst).

---

## Thank you for checking out my repository! Let’s code together! 💻
