---
layout: minimal
title: "Normalisation Task"
---

# Unit 7 – Normalisation Task

## Task
Normalise the below table to **3rd Normal Form (3NF)** showing each step of the process.

---

## Original Table

| Student Number | Student Name | Exam Score | Support | Date of Birth | Course Name       | Exam Boards | Teacher Name |
|----------------|--------------|------------|---------|---------------|------------------|-------------|--------------|
| 1001 | Bob Baker     | 78 | No  | 2001-08-25 | Computer Science | BCS     | Mr Jones   |
|      |              |    |     |            | Maths             | EdExcel | Ms Parker |
|      |              |    |     |            | Physics           | OCR     | Mr Peters |
| 1002 | Sally Davies | 55 | Yes | 1999-10-02 | Maths             | AQA     | Ms Parker |
|      |              |    |     |            | Biology           | WJEC    | Mrs Patel |
|      |              |    |     |            | Music             | AQA     | Ms Daniels |
| 1003 | Mark Hanmill | 90 | No  | 1995-06-05 | Computer Science  | BCS     | Mr Jones   |
|      |              |    |     |            | Maths             | EdExcel | Ms Parker |
|      |              |    |     |            | Physics           | OCR     | Mr Peters |
| 1004 | Anas Ali     | 70 | No  | 1980-08-03 | Maths             | AQA     | Ms Parker |
|      |              |    |     |            | Physics           | OCR     | Mr Peters |
|      |              |    |     |            | Biology           | WJEC    | Mrs Patel |
| 1005 | Cheuk Yin    | 45 | Yes | 2002-05-01 | Computer Science  | BCS     | Mr Jones   |
|      |              |    |     |            | Maths             | EdExcel | Ms Parker |
|      |              |    |     |            | Music             | AQA     | Ms Daniels |

---

## 1st Normal Form (1NF)
Each row is unique and each value is atomic.

| Student Number | Student Name | Exam Score | Support | Date of Birth | Course Name       | Exam Boards | Teacher Name |
|----------------|--------------|------------|---------|---------------|------------------|-------------|--------------|
| 1001 | Bob Baker     | 78 | No  | 2001-08-25 | Computer Science | BCS     | Mr Jones   |
| 1001 | Bob Baker     | 78 | No  | 2001-08-25 | Maths             | EdExcel | Ms Parker |
| 1001 | Bob Baker     | 78 | No  | 2001-08-25 | Physics           | OCR     | Mr Peters |
| 1002 | Sally Davies  | 55 | Yes | 1999-10-02 | Maths             | AQA     | Ms Parker |
| 1002 | Sally Davies  | 55 | Yes | 1999-10-02 | Biology           | WJEC    | Mrs Patel |
| 1002 | Sally Davies  | 55 | Yes | 1999-10-02 | Music             | AQA     | Ms Daniels |
| 1003 | Mark Hanmill  | 90 | No  | 1995-06-05 | Computer Science  | BCS     | Mr Jones   |
| 1003 | Mark Hanmill  | 90 | No  | 1995-06-05 | Maths             | EdExcel | Ms Parker |
| 1003 | Mark Hanmill  | 90 | No  | 1995-06-05 | Physics           | OCR     | Mr Peters |
| 1004 | Anas Ali      | 70 | No  | 1980-08-03 | Maths             | AQA     | Ms Parker |
| 1004 | Anas Ali      | 70 | No  | 1980-08-03 | Physics           | OCR     | Mr Peters |
| 1004 | Anas Ali      | 70 | No  | 1980-08-03 | Biology           | WJEC    | Mrs Patel |
| 1005 | Cheuk Yin     | 45 | Yes | 2002-05-01 | Computer Science  | BCS     | Mr Jones   |
| 1005 | Cheuk Yin     | 45 | Yes | 2002-05-01 | Maths             | EdExcel | Ms Parker |
| 1005 | Cheuk Yin     | 45 | Yes | 2002-05-01 | Music             | AQA     | Ms Daniels |

---

## 2nd Normal Form (2NF)

Must be in 1NF and each column that is not primary key must be dependent on the primary key

Primary key: `Student Number` 

Attributes that depend only on `Course Name` are moved to separate tables.

### Student Details

| Student Number | Student Name  | Exam Score | Support | Date of Birth |
|----------------|---------------|------------|---------|---------------|
| 1001           | Bob Baker     | 78         | No      | 2001-08-25    |
| 1002           | Sally Davies  | 55         | Yes     | 1999-10-02    |
| 1003           | Mark Hanmill  | 90         | No      | 1995-06-05    |
| 1004           | Anas Ali      | 70         | No      | 1980-08-03    |
| 1005           | Cheuk Yin     | 45         | Yes     | 2002-05-01    |

### Course Details

| Course Name       | Exam Boards | Teacher Name |
|-------------------|-------------|--------------|
| Computer Science  | BCS         | Mr Jones     |
| Maths             | EdExcel     | Ms Parker    |
| Physics           | OCR         | Mr Peters    |
| Biology           | WJEC        | Mrs Patel    |
| Music             | AQA         | Ms Daniels   |

### Courses Taken

| Student Number | Course Name       |
|----------------|------------------|
| 1001           | Computer Science |
| 1001           | Maths            |
| 1001           | Physics          |
| 1002           | Maths            |
| 1002           | Biology          |
| 1002           | Music            |
| 1003           | Computer Science |
| 1003           | Maths            |
| 1003           | Physics          |
| 1004           | Maths            |
| 1004           | Physics          |
| 1004           | Biology          |
| 1005           | Computer Science |
| 1005           | Maths            |
| 1005           | Music            |

---

## 3rd Normal Form (3NF)

Must be in 2NF and every column not part of primary key is only dependent on the primary key

Split Exam Boards and Teachers into separate tables (both may cover multiple subjects), link teacher to course and EBID to course

### Student Details (same as for 2NF)

| Student Number | Student Name  | Exam Score | Support | Date of Birth |
|----------------|---------------|------------|---------|---------------|
| 1001           | Bob Baker     | 78         | No      | 2001-08-25    |
| 1002           | Sally Davies  | 55         | Yes     | 1999-10-02    |
| 1003           | Mark Hanmill  | 90         | No      | 1995-06-05    |
| 1004           | Anas Ali      | 70         | No      | 1980-08-03    |
| 1005           | Cheuk Yin     | 45         | Yes     | 2002-05-01    |

### Exam Boards

| EBID | Exam Board |
|------|------------|
| EB1  | BCS        |
| EB2  | EdExcel    |
| EB3  | OCR        |
| EB4  | WJEC       |
| EB5  | AQA        |

### Teachers

| TID | Teacher Name |
|-----|--------------|
| T1  | Mr Jones     |
| T2  | Ms Parker    |
| T3  | Mr Peters    |
| T4  | Mrs Patel    |
| T5  | Ms Daniels   |

### Courses

| CourseID | Course Name      |
|----------|-----------------|
| C1       | Computer Science |
| C2       | Maths            |
| C3       | Physics          |
| C4       | Biology          |
| C5       | Music            |

### Link Teachers to Courses

| Course Name       | TID |
|------------------|-----|
| Computer Science | T1  |
| Maths            | T2  |
| Physics          | T3  |
| Biology          | T4  |
| Music            | T5  |

### Link Exam Boards to Courses

| Course Name       | EBID |
|------------------|------|
| Computer Science | EB1  |
| Maths            | EB2  |
| Physics          | EB3  |
| Biology          | EB4  |
| Music            | EB5  |

---

## Observations / Learnings

- It is not always straightforward to determine what the primary key(s) are and this will determine how the tables are separated
- I reflected that human readability becomes harder as progress through the normal forms, but it is clear that normalisation reduces redundancy and helps prevent inconsistent data
- Careful entry and linking of data is required — mistakes early on propagate through all normal forms.
