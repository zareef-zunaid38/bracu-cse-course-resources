# CSE370 Lab Assignment 1 (Summer 2026)

Single-table MySQL lab assignment managing an inter-university marathon runner database (`runners`), including table creation, sample data insertion, schema modifications, calculated metric updates, and filtering queries.
Docx last modified: 26-06-2026.

## Content

### Overview & Scenario
- Course: CSE370 Database Systems
- Objective: Practice basic SQL operations using a single table in MySQL.
- Scenario: BRAC University is hosting an inter-university marathon event. As part of the organizing committee, manage the runner database on a single table named `runners` under database `Marathon_<Your8DigitStudentID>`.
- Example setup:
  ```sql
  CREATE DATABASE Marathon_12345678;
  USE Marathon_12345678;
  ```

### Part 1: Schema & Data
Table name: `runners`

| student_id | student_name | email | distance_covered | time_taken_mins |
|---|---|---|---|---|
| 101 | Mikasa Ackerman | mikasa.a@g.bracu.ac.bd | 5 | 35 |
| 102 | Edward Elric | e.elric@iub.ac.bd | 10 | 59 |
| 103 | Riza Hawkeye | riza.h@ewu.ac.bd | 5 | 38 |
| 104 | Bruce Wayne | b.wayne@g.bracu.ac.bd | 10 | 63 |
| 105 | Peter Parker | p.parker@iub.ac.bd | 5 | 33 |
| 106 | Alphonse Elric | a.elric@iub.ac.bd | 10 | 55 |
| 107 | Son Goku | son.goku@g.bracu.ac.bd | 5 | 46 |
| 108 | Gon Freecss | gon.f@g.bracu.ac.bd | 10 | 68 |
| 109 | Tanjiro Kamado | tanjiro.k@g.bracu.ac.bd | 5 | 29 |
| 110 | Roy Mustang | roy.m@ewu.ac.bd | 10 | 83 |

*Units:* `distance_covered` in kilometers (km), `time_taken_mins` in minutes.

### Part 2: SQL Tasks (Q1–Q10)
Execute queries in order (provide 1 single query per question):

- **Q1:** Create table `runners` in MySQL with appropriate data types for all columns.
- **Q2:** Insert all 10 records into the `runners` table.
- **Q3:** Display all information of runners whose `distance_covered` is above 6 kilometers.
- **Q4:** Display all runners' `student_name` in uppercase, ordered by their `time_taken_mins` in descending order.
- **Q5:** Change column name `distance_covered` to `total_distance` with data type `DECIMAL(3,1)`.
- **Q6:** Add new column `kmh` with data type `DECIMAL(3,1)` to the table (representing speed in km/h).
- **Q7:** Update `kmh` for each runner using the formula:
  $$\text{kmh} = \frac{\text{total\_distance} \times 60}{\text{time\_taken\_mins}}$$
- **Q8:** Show points of each runner along with `student_id`, `student_name`, and `kmh`. Points formula:
  $$\text{points} = \left(\frac{\text{total\_distance}}{10}\right) \times \left(\frac{\text{time\_taken\_mins}}{60}\right) \times 100$$
- **Q9:** Find `student_name` of runners whose `total_distance` is 10 km and `time_taken_mins` is between 40 and 60 minutes.
- **Q10:** Find `student_name` of runners from BRAC University (filter email domain `@g.bracu.ac.bd`).

### Part 3: Submission Instructions
- Make a copy of the submission template in Google Drive.
- For each task: write the SQL command, execute in XAMPP Shell, capture screenshot showing command and output, paste below command.
- Export document as PDF.
- File naming convention: `ID_Name_CSE370_Lab_Sec##_Assignment01_Summer26.pdf` (e.g., `12345678_PeterParker_CSE370_Lab_Sec12_Assignment01_Summer26.pdf`).
- Submit via Google Form in Discord section channel.
