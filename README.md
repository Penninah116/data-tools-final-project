# My Library Management SQL Project

<a name="readme-top"></a>

<!-- TABLE OF CONTENTS -->

# 📗 Table of Contents

- [My SQL Project](#about-project)
- [📗 Table of Contents](#-table-of-contents)
- [📖 My SQL Project](#about-project)
  - [🛠 Built With ](#-built-with-)
    - [Tech Stack ](#tech-stack-)
    - [Key Features ](#key-features-)
  - [💻 Getting Started ](#-getting-started-)
    - [Prerequisites](#prerequisites)
    - [Setup](#setup)
    - [Usage](#usage)
  - [👥 Authors ](#-authors-)
  - [🔭 Future Features ](#-future-features-)
  - [🤝 Contributing ](#-contributing-)

<!-- Library Management-->

# 📖 My SQL Project <a name="about-project"></a>

**My SQL Project** is a simple Database that uses SQL, Postgres via Supabase and R to create, query and secure a **Library Management** database.

## 🛠 Built With <a name="built-with"></a>

### Tech Stack <a name="tech-stack"></a>
- Supabase
- Postgres DB
  

<!-- Features -->

### Key Features <a name="key-features"></a>

- [ ] **Tables**
- [ ] **Schema**
- [ ] **Access control**

<p align="right">(<a href="#readme-top">back to top</a>)</p>

<!-- GETTING STARTED -->

## 💻 Getting Started <a name="getting-started"></a>

To rebuild this DB, follow these steps.

### Prerequisites

To run this project, you need:
- [A Supabase account](https://supabase.com/)
- [Knowledge on SQL](https://www.w3schools.com/sql/)
- Supabase SQL editor access

<!-- ### Setup -->
### Setup

Copy the contents of this Readme.md to your Project's file

OR

Clone this repository to your desired folder:

```sh
  git clone https://github.com/Penninah116/readme-template-data
  cd budget-app
```

<!-- ### DB Creation -->

### DB Schema

- The DB is made up of 3 tables. Eaach table has 5 entries.
- To create the table, you will need a schema as shown below:

```sql
-- Drop tables if they exist (for re-runs)
DROP TABLE IF EXISTS borrow_records;
DROP TABLE IF EXISTS students;
DROP TABLE IF EXISTS books;

-- Create students table
CREATE TABLE students (
    id SERIAL PRIMARY KEY,
    name VARCHAR(100),
    email VARCHAR(100) UNIQUE,
    enrollment_year INT,
    major VARCHAR(50)
);

-- Create books table
CREATE TABLE books (
    id SERIAL PRIMARY KEY,
    title VARCHAR(150),
    author VARCHAR(100),
    genre VARCHAR(50),
    published_year INT
);

-- Create borrow_records table
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

- The Tables should look like this in Supabase:

books:
<img width="1920" height="1032" alt="image" src="https://github.com/user-attachments/assets/b8c8a94e-29f7-4a9e-9125-19e739353752" >


borrow_records:
<<img width="1920" height="1032" alt="image" src="https://github.com/user-attachments/assets/4d87bc9c-7820-4ba7-80a0-08cbd0937a9c" />


students:
< <img width="1920" height="1032" alt="image" src="https://github.com/user-attachments/assets/7d3bc459-d67d-4883-a4fe-8a3fb042efde" />




- The ERD screenshot from Supabase looks like this: 
<<img width="1920" height="1032" alt="image" src="https://github.com/user-attachments/assets/d77ad4e2-55e1-4770-894b-f929940685aa" />
" />

- To test the table, I used two queries: 

```sql
SELECT s.name AS student_name, b.title AS book_title, br.borrow_date, br.return_date
FROM borrow_records br
JOIN students s ON br.student_id = s.id
JOIN books b ON br.book_id = b.id
WHERE s.name = 'Jane Doe';
````

```sql
SELECT s.name AS student_name, b.title AS book_title, br.borrow_date
FROM borrow_records br
JOIN students s ON br.student_id = s.id
JOIN books b ON br.book_id = b.id
WHERE br.return_date IS NULL;
````

- Here are the results of the queries:
<<img width="1920" height="1032" alt="image" src="https://github.com/user-attachments/assets/897bc92a-bd7e-4e12-850b-bd9956057a70" />
 />

<<img width="1920" height="1032" alt="image" src="https://github.com/user-attachments/assets/5018792e-8137-432b-a373-ade3b3baef3f" />
/p>

<!-- AUTHORS -->

## 👥 Authors <a name="authors"></a>

👤 **Penninah Wambui**

- GitHub: [@Penninah116](https://github.com/penninah116)
- Twitter: [@foiwanjiru_](https://twitter.com/foiwanjiru)
- LinkedIn: [@PenninahWanjiru](https://linkedin.com/in/penninahwanjiru)

<p align="right">(<a href="#readme-top">back to top</a>)</p>

<!-- FUTURE FEATURES -->

## 🔭 Future Features <a name="future-features"></a>

- [ ] **Add security**
- [ ] **Link DB to R for visualisation purposes and further analyses**

<p align="right">(<a href="#readme-top">back to top</a>)</p>

<!-- CONTRIBUTING -->

## 🤝 Contributing <a name="contributing"></a>

Contributions, issues, and feature requests are welcome!

Feel free to check the [issues page](../../issues/).

<p align="right">(<a href="#readme-top">back to top</a>)</p>

<!-- SUPPORT -->
