# CSE370 Lab 01 Sheet (Student View)

Instructional lab sheet for CSE370 Lab 01 covering XAMPP MySQL environment configuration, DDL table definition (`Lab_Grades`), DML record insertion, table alterations, data updates/deletions, and foundational SELECT queries with filtering, ordering, arithmetic expressions, and pattern matching.
Docx last modified: 26-06-2026.

## Content

### Course Metadata
- Department: Department of Computer Science and Engineering, BRAC University
- Course Code: CSE370
- Course Name: Database Systems
- Semester: Summer 2026

---

### Part A: Setting Up and Connecting to MySQL Server
- **Step 1:** Download XAMPP for OS from Apache Friends (`https://www.apachefriends.org/index.html`).
- **Step 2:** Install XAMPP.
- **Step 3:** Open XAMPP Control Panel and start **Apache** and **MySQL** modules.
- **Step 4:** Click the **Shell** button on the right side of the control panel window.
- **Step 5:** Connect to MySQL server:
  ```bash
  mysql -u root -p
  ```
  *(Press Enter when prompted for password; default XAMPP root password is empty string).*

---

### Part B: Introduction to MySQL Queries
- **Stuck prompt / syntax recovery:** If stuck in a multiline prompt (`'>`, `">`, `->`), enter closing characters `');`, `");`, `\`;`, `';` or press `Ctrl+C` to abort.
- **Database listing:**
  ```sql
  SHOW DATABASES;
  ```
- **Database creation:**
  ```sql
  CREATE DATABASE DB_Name;
  ```
- **Selecting database:**
  ```sql
  USE DB_Name;
  ```
- **Table Creation (`Lab_Grades`):**
  ```sql
  CREATE TABLE Lab_Grades (
      std_id CHAR(4),
      name VARCHAR(30),
      major CHAR(3),
      section CHAR(1),
      days_present INT,
      project_marks DOUBLE,
      cgpa DECIMAL(3,2),
      submission_date DATE
  );
  ```
- **Table Insertion:**
  - Explicit column specification:
    ```sql
    INSERT INTO Lab_Grades (std_id, name, major, section, days_present, project_marks, cgpa, submission_date)
    VALUES ('s001', 'Abir', 'CS', '1', 10, 18.5, 3.91, '2018-09-15');
    ```
  - Shorthand syntax:
    ```sql
    INSERT INTO Lab_Grades
    VALUES ('s001', 'Abir', 'CS', '1', 10, 18.5, 3.91, '2018-09-15');
    ```
- **Viewing all data:**
  ```sql
  SELECT * FROM Lab_Grades;
  ```

---

### Part C: SQL Alter, Update, Delete & Basic Select Queries

#### Task 1: Modifying Table Columns
- Add column `project_title`:
  ```sql
  ALTER TABLE Lab_Grades ADD project_title CHAR(10);
  ```
- Modify column data type to `VARCHAR(50)`:
  ```sql
  ALTER TABLE Lab_Grades MODIFY COLUMN project_title VARCHAR(50);
  ```
- Drop column `project_title`:
  ```sql
  ALTER TABLE Lab_Grades DROP COLUMN project_title;
  ```
- Rename column `submission_date` to `sub_date`:
  ```sql
  ALTER TABLE Lab_Grades CHANGE COLUMN submission_date sub_date DATE;
  ```

#### Task 2: Updating Data
- Update single field by condition:
  ```sql
  UPDATE Lab_Grades SET major = 'CSE' WHERE name = 'Arafat';
  ```
- Update multiple fields by primary identifier:
  ```sql
  UPDATE Lab_Grades SET name = 'Naheed', project_marks = 16 WHERE std_id = 's004';
  ```
- *Safety note:* Omitting `WHERE` clause updates all rows across the entire table.

#### Task 3: Deleting Data
- Delete specific student record:
  ```sql
  DELETE FROM Lab_Grades WHERE name = 'Naima';
  ```
- Conditional batch deletion:
  ```sql
  DELETE FROM Lab_Grades WHERE days_present < 8;
  ```

#### Task 4: Dropping Tables and Databases (Destructive)
```sql
DROP TABLE Table_Name;
DROP DATABASE DB_Name;
```

#### Task 5: Retrieving Data (SELECT Queries)
- Retrieve specific columns:
  ```sql
  SELECT std_id, name, project_marks FROM Lab_Grades;
  ```
- Calculated fields and column aliases:
  ```sql
  SELECT name, project_marks + days_present * 5 / 12 AS total_marks FROM Lab_Grades;
  ```
- String case transformation:
  ```sql
  SELECT UPPER(name), LOWER(name) FROM Lab_Grades;
  ```
- Eliminating duplicates:
  ```sql
  SELECT DISTINCT major FROM Lab_Grades;
  ```
- Sorting results:
  - Single column ascending:
    ```sql
    SELECT * FROM Lab_Grades ORDER BY name;
    ```
  - Multiple columns mixed sort:
    ```sql
    SELECT * FROM Lab_Grades ORDER BY name DESC, submission_date ASC;
    ```
- Conditional filtering:
  - Equality:
    ```sql
    SELECT name, project_marks FROM Lab_Grades WHERE major = 'CSE';
    ```
  - Numeric inequality:
    ```sql
    SELECT name, days_present, project_marks FROM Lab_Grades WHERE project_marks > 17;
    ```
  - Range filter with `BETWEEN`:
    ```sql
    SELECT name, project_marks FROM Lab_Grades WHERE project_marks BETWEEN 17 AND 19;
    ```
  - Set membership with `IN`:
    ```sql
    SELECT * FROM Lab_Grades WHERE major IN ('CSE', 'CS');
    ```
  - Compound conditions with date ranges:
    ```sql
    SELECT * FROM Lab_Grades WHERE project_marks > 18 AND submission_date BETWEEN '2018-08-01' AND '2018-08-31';
    ```
- Wildcard pattern matching (`LIKE`):
  - Starts with 'a':
    ```sql
    SELECT * FROM Lab_Grades WHERE name LIKE 'a%';
    ```
  - Contains at least two 'a's:
    ```sql
    SELECT * FROM Lab_Grades WHERE name LIKE '%a%a%';
    ```
  - Starts with 'a' followed by exactly 3 characters (total length 4):
    ```sql
    SELECT * FROM Lab_Grades WHERE name LIKE 'a___';
    ```

#### Task 6: External Quiz
- SQLZoo basic select practice quiz link: `https://sqlzoo.net/wiki/SELECT_Quiz`
