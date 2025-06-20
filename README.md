
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
