# 🎟 Library Management Database – Admin & User Roles in Supabase

<div align="center">
  <img width="314" height="285" alt="Supabase Logo" src="https://github.com/user-attachments/assets/20661293-a214-4004-9042-657102fb0710" />
  <br/>
  <h3><b>Data Fundamentals Project</b></h3>
</div>

---

## 📗 Table of Contents

* [📖 About the Project](#about-project)  
* [🛠 Built With](#built-with)  
* [🚀 Live Demo](#live-demo)  
* [💻 Getting Started](#getting-started)  
* [💾 Sample SQL Queries & Policies](#sample-sql-queries)   
* [🛡 Security Notes](#security-notes)  
* [👥 Authors](#authors)  
* [🔭 Future Features](#future-features)  
* [🤝 Contributing](#contributing)  
* [⭐️ Show your support](#support)  
* [🙏 Acknowledgements](#acknowledgements)  
* [❓ FAQ](#faq)  
* [📝 License](#license)  

---

# 📖 About the Project <a name="about-project"></a>

This project demonstrates an **Library Management Database** implemented using Supabase (PostgreSQL).
It integrates Row Level Security (RLS), Admin & User roles, and custom SQL policies to control data access and enforce least privilege principles.

The System includes:  
- ✅ Tables for students, books, and borrow_records
- ✅ UUID-based authentication via Supabase Auth
- ✅ Role-based access policies (Admin vs Student)
- ✅ Row Level Security (RLS) on all tables
- ✅ SQL functions for admin-only operations

---

## 🛠 Built With <a name="built-with"></a>

- **Supabase** – PostgreSQL + Auth + Policy Management  
- **PostgreSQL** – Structured database engine and tables  
- **RLS Policies** - Fine-grained access control
- **SQL Functions** – Role-based admin actions

---

## 🚀 Live Demo <a name="live-demo"></a>

- [Supabase Dashboard](https://supabase.com/dashboard/project/pwsbzyjjqwxtqzzpaghy)  

---

## 💻 Getting Started <a name="getting-started"></a>

### Prerequisites
- Supabase account
- Basic SQL and PostgreSQL knowledge
-Git installed
- GitHub account for project submission  

### Setup

```bash
git clone https://github.com/Penninah116/library-management-db
cd library-management-db

```

```bash
Usage

Open Supabase SQL editor

Run schema.sql to create tables & sample data

Apply UUID + RLS setup:
Enable Row Level Security
-- Enable RLS
ALTER TABLE students ENABLE ROW LEVEL SECURITY;
ALTER TABLE books ENABLE ROW LEVEL SECURITY;
ALTER TABLE borrow_records ENABLE ROW LEVEL SECURITY;



Apply user vs admin policies.
```

---

## 💾 Sample SQL Queries & Policies <a name="sample-sql-queries"></a>

### 1️⃣ User Policies
```sql
-- Students can view only their own borrow records
CREATE POLICY "Students can view their own borrow records"
ON borrow_records
FOR SELECT
USING (
  student_id = current_setting('request.student_id')::INT
);
```sql
 -- Students can insert their own borrow records
CREATE POLICY "Students can insert their own borrow records"
ON borrow_records
FOR INSERT
WITH CHECK (
  student_id = current_setting('request.student_id')::INT
);

```

```sql
--Restrict Students to Only View Books They've Borrowed
CREATE POLICY "Students can view borrowed books only"
ON books
FOR SELECT
USING (
  EXISTS (
    SELECT 1
    FROM borrow_records
    WHERE borrow_records.book_id = books.id
    AND borrow_records.student_id = current_setting('request.student_id')::INT
  )
);

```
---

### 2️⃣ Admin Policies
```sql
--  Admins can manage all borrow records

CREATE POLICY "Admins have full access to borrow records"
ON borrow_records
FOR ALL
USING (
  EXISTS (
    SELECT 1 FROM students WHERE students.id = borrow_records.student_id AND students.major = 'Admin'
  )
);

```sql
-- Admins can manage all books
CREATE POLICY "Admins have full access to books"
ON books
FOR ALL
USING (
  EXISTS (
    SELECT 1 FROM students WHERE auth_user_id = auth.uid() AND major = 'Admin'
  )
);

```

```sql
--Admins can manage all student records
CREATE POLICY "Admins can manage all students"
ON students
FOR ALL
USING (
  EXISTS (
    SELECT 1 FROM users WHERE users.user_uuid = auth.uid() AND users.role = 'admin'
  )
);

```

---
 ### 3️⃣ Example CRUD Queries with their output.
 ```sql
--List all books currently borrowed (not yet returned)
SELECT b.title, s.name AS borrower, br.borrow_date
FROM borrow_records br
JOIN books b ON br.book_id = b.id
JOIN students s ON br.student_id = s.id
WHERE br.return_date IS NULL;


--List all books borrowed by a specific student (e.g., 'Alice Kim')
SELECT b.title, br.borrow_date, br.return_date
FROM borrow_records br
JOIN books b ON br.book_id = b.id
JOIN students s ON br.student_id = s.id
WHERE s.name = 'Alice Kim';


--View All Borrow Records with Student and Book Details
  SELECT 
  br.id AS borrow_id,
  s.name AS student_name,
  s.email,
  b.title AS book_title,
  b.author,
  br.borrow_date,
  br.return_date
FROM borrow_records br
JOIN students s ON br.student_id = s.id
JOIN books b ON br.book_id = b.id
ORDER BY br.borrow_date DESC;

```
1️⃣Show books currently borrowed by a student
<img width="930" height="454" alt="image" src="https://github.com/user-attachments/assets/59456d64-9d93-46fe-8152-7cb8b92decfc" />


2️⃣List all books borrowed by a specific student (e.g., 'Alice Kim')
<img width="900" height="455" alt="image" src="https://github.com/user-attachments/assets/a6b650a8-1260-4ae6-aa0c-c796347473aa" />

  View All Borrow Records with Student and Book Details
  <img width="884" height="580" alt="image" src="https://github.com/user-attachments/assets/b255601b-9ba9-47f5-a033-fb9f9288ef4a" />


## User Roles and Output

**Student can view all available books in the library**
```sql
SELECT id AS book_id, title, author, genre, published_year
FROM books
ORDER BY title;

```
**View all borrow records with student and book details**
```sql
SELECT br.id AS borrow_id, s.name AS student_name, b.title AS book_title, br.borrow_date, br.return_date
FROM borrow_records br
JOIN students s ON br.student_id = s.id
JOIN books b ON br.book_id = b.id
ORDER BY br.borrow_date DESC;

```
  **Ouput upon Inserting & Viewing Student can view all available books in the library**

<img width="940" height="576" alt="image" src="https://github.com/user-attachments/assets/6e950b59-25de-4eef-82ca-d5c34ff57d85" />

**View all borrow records with student and book details**

<img width="946" height="492" alt="image" src="https://github.com/user-attachments/assets/940093bf-4c08-4cc4-b34c-7ae5e7294209" />


  **User can also delete their purchased ticket**

```sql
DELETE FROM tickets
WHERE customer_id = 1 AND event_id = 1;

```
**User cancels (deletes) their ticket with id=1**

<img width="1900" height="838" alt="image" src="https://github.com/user-attachments/assets/c16dbed7-1f6b-4340-b135-f9592b64ae41" />



## Admin Roles and Output

**Admin can view All Students and Their Majors**

```sql
SELECT 
  id AS student_id,
  name,
  email,
  enrollment_year,
  major
FROM students
ORDER BY name;

```
<img width="851" height="569" alt="image" src="https://github.com/user-attachments/assets/0e18dc1f-fc88-4b28-b57a-92f2c04746fb" />


**Admin can Count of Books Borrowed by Each Student**
```sql
--SELECT s.name AS student_name, COUNT(*) AS total_borrowed
FROM borrow_records br
JOIN students s ON br.student_id = s.id
GROUP BY s.name
ORDER BY total_borrowed DESC; 

```
<img width="892" height="432" alt="image" src="https://github.com/user-attachments/assets/b2840469-f11a-4124-8685-ccf98eafcae3" />

```sql
**Admin can view All Books in the Library**
SELECT 
  id AS book_id,
  title,
  author,
  genre,
  published_year
FROM books
ORDER BY title;

```
<img width="893" height="609" alt="image" src="https://github.com/user-attachments/assets/b611453b-5ee6-4b78-956f-fd336b8c5e74" />

---

## 🛡 Security Notes <a name="security-notes"></a>

See full explanation of RLS, policies, and admin functions in 👉 [security_notes.md](https://github.com/DENNIS-MURITHI/Data-Fundamentals/blob/data_test_branch/security_notes.md)


---

## 👥 Authors <a name="authors"></a>

- **Evans Kibet**  
  GitHub: [@EvansKibet](https://github.com/Evans-dotcom)  
  LinkedIn: [Evans Kibet](https://www.linkedin.com/in/evans-langat-680b05342/)  

---

## 🔭 Future Features <a name="future-features"></a>

- Integrate with front-end event booking portal
- Add analytics for most attended events and top-paying customers
- Implement audit logging for admin actions (create, update, delete events)
- Enable ticket QR code generation for entry validation
-Add email/SMS notifications for successful payments
-Include refund and cancellation management
-Implement role-based access (Admin, Customer)

Add dashboard for revenue and sales insights
---

## 🤝 Contributing <a name="contributing"></a>

Open issues or pull requests are welcome.

---

## ⭐️ Show your support <a name="support"></a>

Give a ⭐️ if you like this project!

---

## 🙏 Acknowledgements <a name="acknowledgements"></a>

- Supabase docs for SQL & RLS policies  
- PostgreSQL official docs  

---

## ❓ FAQ <a name="faq"></a>

**Q: How do I test RLS policies?**  
A: Sign in as User vs Admin and try CRUD operations. Policies will restrict or allow access accordingly.  

**Q: Can I extend this to a front-end?**  
A: Yes, connect Supabase Auth with **Angular**, **Java**, or any front-end framework.  

---

## 📝 License <a name="license"></a>

This project is licensed under the MIT License - see the LICENSE file for details.
