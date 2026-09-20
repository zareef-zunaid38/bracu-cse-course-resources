---
source: CSE370_Lab01_Sheet [Student View].docx (kept alongside)
fidelity: the summary above the divider is paraphrased and may omit detail; the final section 'Full extracted text' is a complete script extraction of the source text (images and diagrams are not transcribed there — 4 embedded image(s); see the summary or the source)
source-mtime: 2026-06-26T19:59:12
generated: summary earlier (2026-09); header and full-text section added 2026-09-20
---
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
- Rename column `submission_date` to `sub_date` *(query supplied by the sidecar author; the sheet only asks "How will you change the name of a column from submission_date to sub_date? [Google it!]")*:
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
  - Numeric inequality *(query supplied by the sidecar author for the sheet's prose task "marks are greater than 17"; the sheet's own example uses `BETWEEN 17 and 19`)*:
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

---

## Full extracted text

<!-- script-extracted 2026-09-20 from the source: all text, table cells, speaker notes, image alt text, link targets, headers/footers, footnotes, comments; list numbering, formatting and images are not reproduced -->

Department of Computer Science and Engineering

| Course Code: CSE370 |

| Course Name: Database Systems | Semester: Summer 2026 |

Part A

Setting Up and Connecting to the MySQL Server

Activity List for Part A

Step 1: Go to https://www.apachefriends.org/index.html <https://www.apachefriends.org/index.html> and download XAMPP for your OS.

Step 2: Install XAMPP according to the installation guide.

Step 3: Open the XAMPP control panel after installation.

Open the control panel and click the start buttons (highlighted in red) beside Apache and MySQL.

Step 4: Click on the “shell” button on the right of the window

Step 5: Connect to the MySQL server

After clicking on the shell, you should see a black window. Type in the following command:

mysql -u root -p

When you are asked for a password, don't type anything just press enter. The default password for xampp is an empty string.

Part B

An Introduction to MySQL Queries

Syntax error in a query might cause the mysql> prompt not to appear after executing the query.

Solutions:

Typing one of the following may solve the problem

');

`);

`;

';

Or log out with ctrl+c and log in again

Activity List for Part B

All commands are shown in the red boxes.

In the green box, write the response you see after entering each query. Also, write the query for cases where you had to make changes.

The part of the query in bold italic are variables, the rest are keywords. Sometimes, you might need to change the variables as per requirement.

All new queries should be typed in the command window after mysql>

  (text box) A Server can have multiple databases, for example, a movie database and a car rental database. So how can you view the list of all databases?

  (text box) SHOW DATABASES;

  (text box) If you want to start a new project you should create your own database. After creating check if the new database is in the list now.

  (text box) CREATE DATABASE DB_Name ;

  (text box) Before storing or manipulating any data, you HAVE to select the database you want to work on. All new command will take effect in selected database.

  (text box) USE DB_Name ;

  (text box) All data are stored in tables. Each table will represent 1 entity, for example students_info, the column of the table will be attributes of the students(e.g. student_id, name, department, cgpa, grad_date) and each row will have information about 1 single student. Each attribute has a pre-defined data type such as int, char etc.

  (text box) CREATE TABLE Lab_Grades

  (text box) (

  (text box) std_id char(4),

  (text box) name varchar(30),

  (text box) major char(3),

  (text box) section char(1),

  (text box) days_present int,

  (text box) project_marks double,

  (text box) cgpa decimal(3,2),

  (text box) submission_date date

  (text box) );

  (text box) SHOW TABLES;

  (text box) You can have many tables in database, e.g student_info, teacher_info, course_info etc. So how to view the list of all tables?

  (text box) You might want to check the structure of a table e.g. what columns are there, what are the data types etc.

  (text box) DESCRIBE Table_Name;

| std_id | name | major | section | days_present | project_marks | cgpa | submission_date |

| s001 | Abir | CS | 1 | 10 | 18.5 | 3.91 | 2018-09-15 |

| s002 | Nafis | CSE | 1 | 12 | 20 | 3.86 | 2018-08-15 |

| s003 | Tasneem | CS | 1 | 8 | 18 | 3.57 | 2018-09-18 |

| s004 | Nahid | ECE | 2 | 7 | 16.5 | 3.25 | 2018-08-20 |

| s005 | Arafat | CS | 2 | 11 | 20 | 4.0 | 2018-09-13 |

| s006 | Tasneem | CSE | 1 | 12 | 17.5 | 3.7 | 2018-08-15 |

| s007 | Muhtadi | ECE | 1 | 10 | 19 | 3.67 | 2018-09-16 |

| S008 | Farhana | CSE | 2 | 6 | 15 | 2.67 | 2018-08-16 |

| s009 | Naima | CSE | 2 | 12 | 20 | 3.7 | 2018-08-14 |

Link for Table Data: https://docs.google.com/document/d/1YYP8YpRP2gEvWFoCkp3rpkZKdR-CEjmunhR_3-9s18Q/ <https://docs.google.com/document/d/1YYP8YpRP2gEvWFoCkp3rpkZKdR-CEjmunhR_3-9s18Q/>

  (text box) Now you want to insert the data above in the table you created. There are two commands: a long version and a shorter one! Insert all the data above in the table.

  (text box) INSERT INTO Table_Name (std_id,name,major, section, days_present,project_marks,cgpa,submission_date) values (‘s001’,’Abir’,’CS’,’1’,10, 18.5, 3.91,’2018-09-15’);

  (text box) INSERT INTO Table_Name values (‘s001’,’Abir’,’CS’,’1’, 10, 18.5, 3.91,’2018-09-15’);

  (text box) So now you want to view all the data you inserted? For that we will use the select query. More on that later!

  (text box) SELECT * FROM Table_Name;

Part C

SQL Alter, Update, Delete & Basic Select Queries

Task 1: Modifying Columns of a Table:

  (text box) Add column project_title in the table

  (text box) ALTER TABLE Lab_Grades add project_title char(10);

  (text box) The data type for Project_title should be varchar(50)

  (text box) ALTER TABLE Lab_Grades MODIFY COLUMN project_title varchar(50);

  (text box) ALTER TABLE Lab_Grades DROP COLUMN project_title;

  (text box) Now let’s delete the column Project_title

How will you change the name of a column from submission_date to sub_date? [Google it!]

Task 2: Updating Wrong Data:

  (text box) UPDATE Lab_Grades SET major = ‘CSE’ WHERE name = ‘Arafat’;

  (text box) Oops! Arafat’s major is actually CSE, so update the value in the table

  (text box) UPDATE Lab_Grades SET name=’Naheed’, project_marks =16 where std_id = ‘s004’ ;

  (text box) Nahid’s name is misspelled and also his project marks should be updated to 16.

What will happen if the where clause is not included in the update query, e.g . if you typed Update Lab_Grades set Major = ‘CSE’;? [Don’t try it now, just write the answer]

Task 3: Deleting Data:

  (text box) Naima dropped out of the course. So, delete her data from the table.

  (text box) DELETE FROM Lab_Grades WHERE Name= ‘Naima’;

What would have happened if there was another student named Naima?

  (text box) DELETE FROM Lab_Grades WHERE days_present < 8;

  (text box) Delete the data of everyone who was less than 8 days present.

Task 4: Deleting Table or Database [DO NOT TRY NOW]:

  (text box) DROP TABLE Table_Name;

  (text box) DROP DATABASE DB_Name;

  (text box) So now if you want to delete a table or database you need the following commands

Task 5: Retrieving Data from Table:

What is the [select * from Lab_grades;] command used for?

  (text box) SELECT std_id, name, project_marks FROM Lab_Grades;

  (text box) Let’s say you want to retrieve only the student id, name and project marks.

  (text box) Retrieve the name and total marks of students out of 25 (project + attendance)

  (text box) SELECT name, project_marks+days_present*5/12 AS total_marks FROM Lab_Grades;

The “as” keyword in the above query is known as an alias. Check out what happens if you remove the “as Total_marks” portion from the above command. State the difference below.

Try the command below, and state what the Upper() and Lower() functions mean.

  (text box) SELECT UPPER(name), LOWER(name) from Lab_Grades;

Try the two commands below. What is the difference and why is the distinct keyword used?

  (text box) SELECT DISTINCT major FROM Lab_Grades;

  (text box) SELECT major FROM Lab_Grades;

  (text box) SELECT * FROM Lab_Grades ORDER BY name;

  (text box) Now you want to view all the details sorted by name. You can use the order by keyword

Was it sorted in ascending or descending order? How can you sort in the opposite order?[Hint: check next command]

  (text box) SELECT * FROM Lab_Grades ORDER BY name DESC, submission_date ASC;

  (text box) Sort all details according to name and then by submission date. There are two students named Tasneem, observe what happens.

  (text box) SELECT name,project_marks FROM Lab_Grades WHERE major=’CSE’ ;

  (text box) Now, you want to view the name and project marks for only CSE students.

Retrieve the names, days present and marks of students whose project marks are greater than 17

  (text box) SELECT name,project_marks FROM Lab_Grades WHERE project_marks BETWEEN 17 and 19 ;

  (text box) SELECT * FROM Lab_Grades WHERE major in (‘CSE’, ‘CS’);

  (text box) Retrieve the name and marks of students whose marks is between 17 and 19

  (text box) Retrieve the details of students who are majoring in either CS or CSE

What is the “in” keyword in the above query? In the where clause, you can write the same command using the “or” and “=” operators. Try to figure it out!

  (text box) Retrieve the details of the students who submitted their project in August and whose marks is greater than 18

  (text box) SELECT * FROM Lab_Grades WHERE project_marks>18 and submission_date BETWEEN ‘2018-08-01’ and ‘2018-08-31’;

How can you find the students whose Submission_date is not in August?

  (text box) SELECT * FROM Lab_Grades WHERE name like ‘a%’;

  (text box) Retrieve the details of students whose name start with ‘a’

  (text box) Retrieve the details of students whose name contains at least 2 a’s

  (text box) SELECT * FROM Lab_Grades WHERE name LIKE ‘%a%a%’;

Try the following command and explain what happens : Select * from Lab_Grades where Name like ‘a___’; [There are 3 underscores]

Task 6: Basic Select Quiz

Go to https://sqlzoo.net/wiki/SELECT_Quiz <https://sqlzoo.net/wiki/SELECT_Quiz> and answer the Quiz to test your knowledge of basic select queries.

[footer1.xml] Page of
