# Data Dictionary for Library Management Database 

 This data dictionary describes the structure and purpose of the database tables used in the Library Management project. It defines each table, its columns, data types, and relationships to help developers and analysts clearly understand the data model. 

---
 
## Table: books
This table stores details aboout all the books available in the library including its author, genre and year of publication.
 
<<img width="1304" height="446" alt="image" src="https://github.com/user-attachments/assets/6a92ec12-b960-4e66-a15f-7c2254abaefe" />
>

---
 
## Table: borrow_records
Contains details about the records for the books that have been borrowed 
 <<img width="1033" height="349" alt="image" src="https://github.com/user-attachments/assets/8e16ee4e-705b-4eaa-b292-b1083faab4ba" />
/>
 
---
 
## Table: Students
The table stores information about students who registered in the library system, including their name, contact email, year of enrollment and major area of study.
<<img width="1049" height="415" alt="image" src="https://github.com/user-attachments/assets/caf8fdf7-0122-4980-8d8e-bb0e62e4807e" />
 

---
 

---
 **Notes:**  
- Primary keys uniquely identify rows in each table.
Foreign keys (FK) connect relationships across tables (books, borrow_records, students).
This schema supports queries like:
- View all books in the library
- Update return date when a book is returned
-Find the most borrowed books 
- List all books that haven’t been returned yet
and many more queries based on your criteria. 
