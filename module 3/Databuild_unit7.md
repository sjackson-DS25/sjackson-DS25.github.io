
# Unit 7 Database Example

Task: 
- Build a relational database system (based on the unit 7 normalisation task), with linked tables.
- Demonstrate knowledge of:
  - Primary keys  
  - Secondary (foreign) keys  
- Test the database to ensure referential integrity  

Th database was built in SQL using the script below.
Referential integrity was then checked by running test queries to test that foreign key constraints and uniqueness constraints were correctly enforced

Reflections/learnings

This task took me some time to complete as I needed to develop the script in SQL, in which I previously had very minimal experience.  I did find however that SQL is a 'easy to understand' programming language and in builing my experiencce in SQL, and undertaking this task which involved producing several table, appropriate commands are fairly easily learnt and recalled.  I made slight updates to the table from the normalisation task, specifically I updated course names in the 'courses_studied, course_teachers and course_examboards' with relevent ID's for simplicity.  
The checks carried out (see end of script) all behaved as expected suggesting that referencial integirty is enforced as required, although the checks were non-exhaustive and additional checks could be undertaken if time allowed to increase confidence in this. 
I realised that  careful planning with regards to data entry and table formatting, together with accurate identification of the primary and foreign key(s) is vital, and this is something I will continue to study in order to gain further experience and confidence.  I additionally reflected that on building larger, more complex databases there could be multiple potential points of error in data entry, and therefore carefull design and set up of the database is crucial.  

```sql
-- Create the database
CREATE DATABASE unit7_db;
USE unit7_db;

-- create tables as per normalisation task (unit 7)

CREATE TABLE Students (
    student_no INT PRIMARY KEY,
    student_name VARCHAR(50),
    exam_score INT,
    support VARCHAR(3), 
    date_of_birth DATE
);

CREATE TABLE ExamBoards (
    ebid VARCHAR(5) PRIMARY KEY,
    board_name VARCHAR(50)
);

CREATE TABLE Teachers (
    tid VARCHAR(5) PRIMARY KEY,
    teacher_name VARCHAR(50)
);

CREATE TABLE Courses (
    course_id VARCHAR(5) PRIMARY KEY,
    course_name VARCHAR(50)
);

CREATE TABLE courses_studied (
    student_no INT,
    course_id VARCHAR(5),
    PRIMARY KEY (student_no, course_id),
    FOREIGN KEY (student_no) REFERENCES students(student_no),
    FOREIGN KEY (course_id) REFERENCES Courses(course_id)
);

CREATE TABLE Course_Teachers (
    course_id VARCHAR(5),
    tid VARCHAR(5),
    PRIMARY KEY (course_id, tid),
    FOREIGN KEY (course_id) REFERENCES Courses(course_id),
    FOREIGN KEY (tid) REFERENCES Teachers(tid)
);

CREATE TABLE Course_ExamBoards (
    course_id VARCHAR(5),
    ebid VARCHAR(5),
    PRIMARY KEY (course_id, ebid),
    FOREIGN KEY (course_id) REFERENCES Courses(course_id),
    FOREIGN KEY (ebid) REFERENCES ExamBoards(ebid)
);

-- populate tables with the data (see unit 7 normalisation task)
INSERT INTO Students VALUES
(1001,'Bob Baker',78,'No','2001-08-25'),
(1002,'Sally Davies',55,'Yes','1999-10-02'),
(1003,'Mark Hanmill',90,'No','1995-06-05'),
(1004,'Anas Ali',70,'No','1980-08-03'),
(1005,'Cheuk Yin',45,'Yes','2002-05-01');

INSERT INTO ExamBoards VALUES
('EB1','BCS'),
('EB2','EdExcel'),
('EB3','OCR'),
('EB4','WJEC'),
('EB5','AQA');

INSERT INTO Teachers VALUES
('T1','Mr Jones'),
('T2','Ms Parker'),
('T3','Mr Peters'),
('T4','Mrs Patel'),
('T5','Ms Daniels');

INSERT INTO Courses VALUES
('C1','Computer Science'),
('C2','Maths'),
('C3','Physics'),
('C4','Biology'),
('C5','Music');

INSERT INTO Course_Teachers VALUES
('C1','T1'),
('C2','T2'),
('C3','T3'),
('C4','T4'),
('C5','T5');

INSERT INTO Course_ExamBoards VALUES
('C1','EB1'),
('C2','EB2'),
('C3','EB3'), 
('C4','EB4'),
('C5','EB5');

INSERT INTO courses_studied VALUES
(1001,'C1'),
(1001,'C2'),
(1001,'C3'),
(1002,'C2'),
(1002,'C4'),
(1002,'C5'),
(1003,'C1'),
(1003,'C2'),
(1003,'C3'),
(1004,'C2'),
(1004,'C3'),
(1004,'C4'),
(1005,'C1'),
(1005,'C2'),
(1005,'C5');

-- ==================================================
-- Test referential integrity
-- ==================================================

-- 1. Try to add a new exam board in 'course_examboards' table
INSERT INTO course_examboards (course_id, ebid) VALUES ('C1', 'EB6');

-- Expected error:
-- Error Code: 1452. Cannot add or update a child row: 
-- a foreign key constraint fails (`unit7_db`.`course_examboards`, 
-- CONSTRAINT `course_examboards_ibfk_2` FOREIGN KEY (`ebid`) 
-- REFERENCES `examboards` (`ebid`))

-- 2. Try deleting a teacher from teachers
DELETE FROM Teachers WHERE tid = 'T1'; 

-- Expected error:
-- Error Code: 1451. Cannot delete or update a parent row: 
-- a foreign key constraint fails (`unit7_db`.`course_teachers`, 
-- CONSTRAINT `course_teachers_ibfk_2` FOREIGN KEY (`tid`) 
-- REFERENCES `teachers` (`tid`))

-- 3. Check uniqueness constraints by re-adding a student/course combo
INSERT INTO courses_studied (student_no, course_id) VALUES (1001, 'C1');

-- Expected error:
-- Error Code: 1062. Duplicate entry '1001-C1' for key 'courses_studied.PRIMARY'

-- 4. Try deleting a student who is still referenced
DELETE FROM Students WHERE student_no = 1001; 

-- Expected error:
-- Error Code: 1451. Cannot delete or update a parent row: 
-- a foreign key constraint fails (`unit7_db`.`courses_studied`, 
-- CONSTRAINT `courses_studied_ibfk_1` FOREIGN KEY (`student_no`) 
-- REFERENCES `students` (`student_no`))
