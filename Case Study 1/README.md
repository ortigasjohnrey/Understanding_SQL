# Student Enrollment Database  
**Author:** JOHN REY ORTIGAS BSCS 3B  

## 📚 Table of Contents
- [Business Task]
- [Entity Relationship Diagram]
- [Question and Solution]



***

## Business Task
This database is designed to manage student enrollment and academic records.

Main tasks it supports:

1. Store student information (ID, first name, last name).

2. Store staff details (ID, name, birthdate).

3. Manage available classes (code, name, units).

4. Track student enrollments:

  - Which student enrolled in which class.

  - Which staff teaches the class.

  - What grade the student received.

Allow queries to:

  - Filter by grade.

  - Search students by name.

  - List students with their classes and grades.

  - Order grades ascending/descending.

***

## Entity Relationship Diagram

![image](https://user-images.githubusercontent.com/81607668/127271130-dca9aedd-4ca9-4ed8-b6ec-1e1920dca4a8.png)

***

## Question and Solution

Please join me in executing the queries using PostgreSQL on [DB Fiddle](https://www.db-fiddle.com/f/d5nSt8eEXi4jpM4z2wXYtv/4). It would be great to work together on the questions!

Additionally, I have also published this case study on [Medium](https://katiehuangx.medium.com/8-week-sql-challenge-case-study-week-1-dannys-diner-2ba026c897ab?source=friends_link&sk=ed355696f5a70ff8b3d5a1b905e5dabe).

If you have any questions, reach out to me on [LinkedIn](https://www.linkedin.com/in/katiehuangx/).

**1. Which students received specific grades (1.3, 1.5, 1.7, 2.0, 3.0)?**

````sql
SELECT *
FROM Enrollment
where gradein (1 .3, 1.5, 1.7, 2.0, 3.0);

````

#### Steps:
- Use a WHERE ... IN clause to filter rows in the Enrollment table.
- Return all enrollment records that match any of the specified grade values.

#### Answer:
| enrollCode | studID     | staffNo | classCode | grade |
| ---------- | ---------- | ------- | --------- | ----- |
| 2018-1025  | 2018-2001A | S-1234  | CS-1      | 1.5   |
| 2019-1027  | 2019-0001A | S-1234  | CS-1      | 1.7   |
| 2019-1121  | 2018-2001A | S-1004  | ICT-104   | 1.5   |
| 2020-1345  | 2018-0001A | S-1234  | CS-1      | 1.3   |
| 2020-1346  | 2019-0002A | S-1234  | CS-1      | 1.3   |

- Only students with those grade values will be shown.

***

**2. Which students have a first or last name starting with "C"?**

````sql
SELECT * 
FROM Student 
WHERE studFname LIKE 'C%' OR studLname LIKE 'C%';

````

#### Steps:
 - Apply a LIKE 'C%' filter to both studFname and studLname.
 - Use OR so that either first name OR last name beginning with "C" qualifies.

#### Answer:
| studID     | studLname | studFname |
| ---------- | --------- | --------- |
| 2019-0001A | Cruz      | Juan      |
| 2020-0001A | Cortez    | Salvacion |

- Lists all students whose names begin with C.

***

**3. Which students got grades between 1.5 and 2.0 and what class did they take?**

````sql
SELECT 
  studFname, 
  studLname, 
  className 
FROM Student, Class, Enrollment 
WHERE Enrollment.studID = Student.studID 
  AND grade BETWEEN 1.5 AND 2.0;

````

#### Steps:
- Use a multi-table join (Student, Class, Enrollment).
- Match students and their enrollments with classes.
- Filter rows where grade falls between 1.5 and 2.0 inclusive.

#### Answer:
| studFname | studLname | className                 |
| --------- | --------- | ------------------------- |
| Juan      | Cruz      | Programming               |
| Juan      | Cruz      | Introduction to Computing |
| Juan      | Cruz      | Intermediate Programming  |
| Juan      | Cruz      | Multimedia Programming    |
| Juan      | Cruz      | Information Management    |

- Shows students and classes where the student achieved a grade in the given range.

***

**4. What are the enrollment records sorted by grade (lowest first)?**

````sql
SELECT 
  studID, 
  classCode, 
  grade 
FROM Enrollment 
ORDER BY grade ASC;

````

#### Steps:
- Select studID, classCode, and grade.
- Sort records in ascending order using ORDER BY grade ASC.

#### Answer:
| studID     | classCode | grade |
| ---------- | --------- | ----- |
| 2020-0001A | ICT-102   | 1.1   |
| 2018-2001A | ICT-102   | 1.2   |
| 2018-0001A | CS-1      | 1.3   |
| 2019-0002A | CS-1      | 1.3   |
| 2019-0001A | ICT-102   | 1.4   |
| 2018-2001A | ICT-109   | 1.4   |
| 2018-2001A | CS-1      | 1.5   |
| 2018-2001A | ICT-104   | 1.5   |
| 2019-0002A | ICT-102   | 1.6   |
| 2019-0002A | ICT-104   | 1.6   |
| 2019-0001A | CS-1      | 1.7   |
| 2019-0002A | ICT-104   | 1.8   |
| 2019-0002A | ICT-104   | 1.8   |
| 2019-0002A | CS-1      | 1.9   |


- Displays students with their grades starting from the lowest grade.

***

**5. List students with their grades sorted from highest to lowest.**

````sql
SELECT 
  studFname, 
  studLname, 
  grade 
FROM Enrollment, Student 
WHERE Enrollment.studID = Student.studID 
ORDER BY grade DESC;

````

#### Steps:
- Join Enrollment and Student tables via studID.
- Select student names along with their grades.
- Order results in descending order to display highest grades first.

#### Answer:
| studFname | studLname | grade |
| --------- | --------- | ----- |
| Juan      | Cruz      | 1.7   |
| Juan      | Cruz      | 1.4   |
| Salvacion | Cortez    | 1.1   |

- Students are listed from highest to lowest grade.
***
