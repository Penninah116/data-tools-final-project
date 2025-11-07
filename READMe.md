# Data-Analysis

<div align="center">
  <img width="200" height="200" alt="Library Management Logo" src="https://github.com/user-attachments/assets/20661293-a214-4004-9042-657102fb0710" />
  <br/>
  <h2><b>Library Management Data Analysis Project</b></h2>
</div>

## 📗 Table of Contents
- 📖 About the Project
- 🛠 Built With
- Key Features
- 🚀 Live Demo
- 💻 Getting Started
- Prerequisites
- Setup
- Usage
- Connecting from Posit to Supabase
- 💾 Schema SQL
- 📊 R Data Analysis
- 📖 Data Dictionary
- 👥 Authors
- 🔭 Future Features
- 🤝 Contributing
- ⭐️ Show your support
- 🙏 Acknowledgements
- ❓ FAQ
- 📝 License

---

## 📖 About the Project <a name="about-project"></a>

This project is a **Library Management Data Analysis System** built in **R (Posit)**, with **Supabase** as the backend database.  
It allows tracking of students, books, and borrowing activity to identify reading trends, book demand, and student engagement.  
The data is analyzed in R using **dplyr** and **ggplot2** for insight visualization.

---

## 🛠 Built With <a name="built-with"></a>

### Tech Stack

<details>
  <summary>Database & Hosting</summary>
  <ul>
    <li><a href="https://supabase.com">Supabase (PostgreSQL)</a> – backend for table storage and querying</li>
  </ul>
</details>

<details>
  <summary>SQL Queries</summary>
  <ul>
    <li>Database schema creation and sample records insertion</li>
    <li>Joins between students, books, and borrow_records for analysis</li>
  </ul>
</details>

<details>
  <summary>R Data Analysis</summary>
  <ul>
    <li><a href="https://posit.co/">Posit / RStudio</a> for connecting to Supabase and analyzing data</li>
    <li>Libraries: DBI, RPostgres, dplyr, ggplot2</li>
  </ul>
</details>

---

## Key Features <a name="key-features"></a>

✅ Manage students, books, and borrowing data  
✅ Analyze borrowing patterns over time  
✅ Identify most borrowed books and active students  
✅ Visualize trends in borrowing and book returns  
✅ Secure data connection via Supabase  

<p align="right"><a href="#about-project">back to top</a></p>

---

## 🚀 Live Demo <a name="live-demo"></a>

Backend-only project. Interact through **Supabase SQL editor** or **RStudio (Posit)**.

Supabase Project Link

<p align="right"><a href="#about-project">back to top</a></p>

---

## 💻 Getting Started <a name="getting-started"></a>

### Prerequisites

- Supabase account  
- Posit / RStudio  
- R packages: DBI, RPostgres, dplyr, ggplot2  

### Setup

```bash
git clone https://github.com/DENNIS-MURITHI/Library-Data-Analysis.git
cd Library-Data-Analysis
```

### Usage

Run SQL scripts in Supabase → SQL Editor → Create tables and insert sample data.  
Then use RStudio to connect and analyze data.

#### Example Connection Script

```r
library(DBI)
connect_db <- function() {
  dbConnect(
    RPostgres::Postgres(),
    dbname = "postgres",
    host = "aws-1-eu-north-1.pooler.supabase.com",
    port = 5432,
    user = "postgres.pwsbzyjjqwxtqzzpaghy",
    password = "usA-wt4/$Gg4x#m",
    sslmode = "require"
  )
}
```

#### Connecting from Posit to Supabase

```r
install.packages(c("DBI", "RPostgres", "dplyr", "ggplot2"))
source("connect_db.R")
con <- connect_db()
dbListTables(con)
```

---

## 💾 Schema SQL <a name="schema-sql"></a>

<details>
<summary>Click to expand full schema.sql</summary>

```sql
-- Drop tables if they exist
DROP TABLE IF EXISTS borrow_records;
DROP TABLE IF EXISTS students;
DROP TABLE IF EXISTS books;

-- Create tables
CREATE TABLE students (
    id SERIAL PRIMARY KEY,
    name VARCHAR(100),
    email VARCHAR(100) UNIQUE,
    enrollment_year INT,
    major VARCHAR(50)
);

CREATE TABLE books (
    id SERIAL PRIMARY KEY,
    title VARCHAR(150),
    author VARCHAR(100),
    genre VARCHAR(50),
    published_year INT
);

CREATE TABLE borrow_records (
    id SERIAL PRIMARY KEY,
    student_id INT REFERENCES students(id),
    book_id INT REFERENCES books(id),
    borrow_date DATE,
    return_date DATE
);

-- Insert sample students
INSERT INTO students (name, email, enrollment_year, major) VALUES
('Jane Doe', 'jane.doe@example.com', 2022, 'Literature'),
('John Smith', 'john.smith@example.com', 2021, 'History'),
('Alice Kim', 'alice.kim@example.com', 2023, 'Computer Science'),
('Mohamed Ali', 'mohamed.ali@example.com', 2020, 'Philosophy'),
('Grace Njeri', 'grace.njeri@example.com', 2022, 'Library Science');

-- Insert sample books
INSERT INTO books (title, author, genre, published_year) VALUES
('1984', 'George Orwell', 'Dystopian', 1949),
('To Kill a Mockingbird', 'Harper Lee', 'Classic', 1960),
('The Great Gatsby', 'F. Scott Fitzgerald', 'Classic', 1925),
('Sapiens', 'Yuval Noah Harari', 'Non-fiction', 2011),
('The Hobbit', 'J.R.R. Tolkien', 'Fantasy', 1937);

-- Insert sample borrow records
INSERT INTO borrow_records (student_id, book_id, borrow_date, return_date) VALUES
(1, 2, '2025-10-01', '2025-10-10'),
(2, 1, '2025-09-25', NULL),
(3, 5, '2025-10-05', '2025-10-12'),
(4, 3, '2025-10-03', NULL),
(5, 4, '2025-10-02', '2025-10-09');
```
</details>

---

## 📊 R Data Analysis <a name="r-data-analysis"></a>

<details>
<summary>Click to expand full R analysis code</summary>

```r
source("connect_db.R")
library(DBI)
library(dplyr)
library(ggplot2)

con <- connect_db()

# 1. Most Borrowed Books
most_borrowed <- dbGetQuery(con, "
  SELECT b.title, COUNT(r.book_id) AS times_borrowed
  FROM borrow_records r
  JOIN books b ON r.book_id = b.id
  GROUP BY b.title
  ORDER BY times_borrowed DESC;
")
ggplot(most_borrowed, aes(x = reorder(title, times_borrowed), y = times_borrowed, fill = title)) +
  geom_col(show.legend = FALSE) +
  coord_flip() +
  labs(title = 'Most Borrowed Books', x = 'Book Title', y = 'Times Borrowed') +
  theme_minimal()
```
</details>

---

## 📊 Dashboard Preview

📊 *(Power BI or ggplot visualization can be embedded here)*  
🖼️ *Placeholder for Library Analytics Dashboard Screenshot*

---

## 📖 Data Dictionary <a name="data-dictionary"></a>

| Table | Column | Description |
|--------|---------|-------------|
| **students** | id | Unique student ID |
|  | name | Student’s full name |
|  | email | Student’s email address |
|  | enrollment_year | Year of joining |
|  | major | Field of study |
| **books** | id | Unique book ID |
|  | title | Book title |
|  | author | Book author |
|  | genre | Book genre |
|  | published_year | Year published |
| **borrow_records** | id | Borrow transaction ID |
|  | student_id | Linked student |
|  | book_id | Linked book |
|  | borrow_date | Borrow date |
|  | return_date | Return date (nullable) |

---

## 👥 Authors <a name="authors"></a>

👤 **Penninah Wanjiru**  
* GitHub: [@Penninah](https://github.com/Penninah116)  
* LinkedIn: [LinkedIn](https://linkedin.com)

---

## 🔭 Future Features <a name="future-features"></a>

- Integrate a front-end for book management  
- Add overdue notifications  
- Expand data analysis to reading frequency and student habits  
- Connect Power BI dashboard for trend visualization  

---

## 🤝 Contributing <a name="contributing"></a>

Contributions are welcome! Fork the repository, create a feature branch, and submit a pull request.

---

## ⭐️ Show your support <a name="support"></a>

If you like this project, give it a ⭐️ on GitHub and share your insights!

---

## 🙏 Acknowledgements <a name="acknowledgements"></a>

- Supabase – PostgreSQL backend  
- Posit / RStudio – Data analysis environment  

---

## ❓ FAQ <a name="faq"></a>

**1. How do I connect R to Supabase?**  
Use DBI + RPostgres as shown in the connection code.

**2. Can I run this locally without Supabase?**  
Yes, by hosting PostgreSQL locally and replacing the connection credentials.

---

## 📝 License <a name="license"></a>

This project is licensed under the **MIT License** – see [LICENSE](LICENSE) for details.
