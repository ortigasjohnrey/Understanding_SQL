# Art Gallery Sales and Inventory Database
Author: JOHN REY ORTIGAS BSCS

📚 Table of Contents

Business Task

This database manages student enrollments, staff assignments, and grading.

Main tasks supported:

1. maintain student information (ID, first name, last name).

2. Store staff details (ID, name, birthdate).

3. Manage subject offerings (class code, name, units).

4. Track enrollments:

 - Student enrolled per class.

 - Staff handling the class.

 - Grade earned by the student.

5. Provide analytical queries:

 - List students with specific grades.

 - Find student details by last name.

 - Retrieve students enrolled in a specific class.

 - Show student grades per class.

***

## Entity Relationship Diagram

![image](https://user-images.githubusercontent.com/81607668/127271130-dca9aedd-4ca9-4ed8-b6ec-1e1920dca4a8.png)

***

## Question and Solution

Please join me in executing the queries using PostgreSQL on https://www.db-fiddle.com/f/tBgq43N8Phjdix7vbMfgET/9. It would be great to work together on the questions!



**1. List students with grades 1.1 or 1.5**

````sql
SELECT studID, classCode, grade 
FROM Enrollment 
WHERE grade = 1.1 OR grade = 1.5;

````
#### Steps:
 - Filter the Enrollment table for grades equal to 1.1 or 1.5.
 - Retrieve student ID, class code, and grade.
#### Answer:
| studID     | classCode | grade |
| ---------- | --------- | ----- |
| 2018-2001A | CS-1      | 1.5   |
| 2018-2001A | ICT-104   | 1.5   |
| 2020-0001A | ICT-102   | 1.1   |


- Shows students who achieved top or high grades.

***

**2. Find student details with last name 'Santos'**

````sql
SELECT studID, studFname, studLname 
FROM Student 
WHERE studLname = 'Santos';

````

#### Steps:

 - Filter the Student table by last name 'Santos'.
 - Retrieve student ID, first name, and last name.
#### Answer:
| studID     | studFname | studLname |
| ---------- | --------- | --------- |
| 2020-0002A | Joselito  | Santos    |

- Provides details of the student with last name Santos.

***

**3. List students enrolled in ICT-102**

````sql
SELECT Student.studFname, Student.studLname 
FROM Enrollment, Student 
WHERE Enrollment.classCode = 'ICT-102' 
  AND Student.studID = Enrollment.studID;

````

#### Steps:
 - Join Student and Enrollment on studID.
 - Filter for class code 'ICT-102'.

#### Answer:
| studFname | studLname |
| --------- | --------- |
| Antonio   | Pastol    |
| Kristina  | Alayon    |
| Juan      | Cruz      |
| Salvacion | Cortez    |


 - Shows all students enrolled in Introduction to Computing.
   
***
**4. Show student grades per class**


````sql
SELECT Student.studFname, Student.studLname, Class.classCode, Enrollment.grade 
FROM Student, Enrollment, Class 
WHERE Student.studID = Enrollment.studID 
  AND Class.classCode = Enrollment.classCode;


````

#### Steps:

 - Join Student, Enrollment, and Class tables.
 - Retrieve student names, class codes, and grades.

#### Answer:
| studFname | studLname | classCode | grade |
| --------- | --------- | --------- | ----- |
| Antonio   | Pastol    | ICT-102   | 1.2   |
| Antonio   | Pastol    | CS-1      | 1.5   |
| Antonio   | Pastol    | ICT-104   | 1.5   |
| Antonio   | Pastol    | ICT-109   | 1.4   |
| Juan      | Cruz      | ICT-102   | 1.4   |
| Juan      | Cruz      | CS-1      | 1.7   |
| Kristina  | Alayon    | ICT-102   | 1.6   |
| Kristina  | Alayon    | CS-1      | 1.9   |
| Kristina  | Alayon    | ICT-104   | 1.6   |
| Kristina  | Alayon    | ICT-104   | 1.8   |
| Salvacion | Cortez    | ICT-102   | 1.1   |
| Salvacion | Cortez    | CS-1      | 1.3   |

 -Provides a full grade report of students per class.
***
