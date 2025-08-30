# Student Enrollment Management Database
Author: JOHN REY ORTIGAS BSCS

📚 Table of Contents

Business Task

Entity Relationship Diagram
Question and Solution
Business Task

This database manages student enrollments, staff assignments, and grading.

Main tasks supported:

1. Maintain student information (ID, first name, last name).

2. Store staff details (ID, name, birthdate).

3. Manage subject offerings (class code, name, units).

4. Track enrollments:

  Student enrolled per class.
  
  Staff handling the class.
  
  Grade earned by the student.

5. Provide analytical queries:

  Count subjects available.
  
  Retrieve highest/lowest grades.
  
  Compute GPA and total grade of a student.

***

## Entity Relationship Diagram

![image](https://user-images.githubusercontent.com/81607668/127271130-dca9aedd-4ca9-4ed8-b6ec-1e1920dca4a8.png)

***

## Question and Solution

Please join me in executing the queries using PostgreSQL on [DB Fiddle](https://www.db-fiddle.com/f/cZ2XuJeRvmDMFjP1KZBEdB/4). It would be great to work together on the questions!


If you have any questions, reach out to me on [LinkedIn]

**1. How many subjects are currently available?**

````sql
SELECT COUNT(DISTINCT className) as AVAILABLE_SUBJECTS 
FROM Class;

````

#### Steps:
- Use COUNT(DISTINCT ...) to ensure unique subjects are counted.
- Query only from the Class table.

#### Answer:
| AVAILABLE_SUBJECTS |
| ------------------ |
| 5                  |

- Shows available subjects to be enrolled

***

**2. What is the highest and lowest grade of student 2018-2001A?**

````sql
SELECT 
  MIN(grade) as HIGHEST_GRADE, 
  MAX(grade) as LOWEST_GRADE 
FROM Enrollment 
WHERE Enrollment.studID = '2018-2001A';

````

#### Steps:
 - Filter records for student 2018-2001A.
 - Use MIN() to capture the best grade (lowest value since 1.0 is highest).
 - Use MAX() to capture the worst grade.

#### Answer:
| HIGHEST_GRADE | LOWEST_GRADE |
| ------------- | ------------ |
| 1.2           | 1.5          |

- Shows the highest and lowest grade recorded

***

**3. What is the GPA and total grade for student 2020-001A?**

````sql
SELECT 
  AVG(grade) as GPA, 
  SUM(grade) as TOTAL_GRADE 
FROM Enrollment 
WHERE Enrollment.studID = '2020-001A';
````

#### Steps:
- Filter enrollment records for the given student.
- Use AVG() to compute GPA.
- Use SUM() to compute total grade.

#### Answer:
| GPA | TOTAL_GRADE |
| --- | ----------- |
|     |             |

- Shows none complied the conditions.

***

| Salvacion | Cortez    | 1.1   |

- Students are listed from highest to lowest grade.
***
