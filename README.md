-- ==========================================
-- LAB 5 : ORACLE FUNCTIONS AND CURSOR
-- ==========================================

-- STUDENT TABLE
CREATE TABLE Student (
    SID NUMBER PRIMARY KEY,
    sName VARCHAR2(50),
    GPA NUMBER(3,2),
    sizeHS NUMBER
);

INSERT INTO Student VALUES (1, 'Rahul', 8.5, 500);
INSERT INTO Student VALUES (2, 'Aman', 7.8, 600);
INSERT INTO Student VALUES (3, NULL, 9.1, 450);
INSERT INTO Student VALUES (4, 'Priya', 8.9, 550);
INSERT INTO Student VALUES (5, 'Neha', 7.5, 700);


-- COLLEGE TABLE
CREATE TABLE College (
    cName VARCHAR2(50) PRIMARY KEY,
    state VARCHAR2(30),
    enrollment NUMBER
);

INSERT INTO College VALUES ('IIT Delhi', 'Delhi', 12000);
INSERT INTO College VALUES ('IIT Bombay', 'Maharashtra', 15000);
INSERT INTO College VALUES ('IIT Indore', 'Madhya Pradesh', 8000);
INSERT INTO College VALUES ('IIT Kanpur', 'Uttar Pradesh', 10000);
INSERT INTO College VALUES ('IIT Madras', 'Tamil Nadu', 14000);


-- APPLY TABLE
CREATE TABLE Apply (
    SID NUMBER,
    cName VARCHAR2(50),
    major VARCHAR2(50),
    decision VARCHAR2(20),

    FOREIGN KEY (SID) REFERENCES Student(SID),
    FOREIGN KEY (cName) REFERENCES College(cName)
);

INSERT INTO Apply VALUES (1, 'IIT Delhi', 'Computer Science', 'Y');
INSERT INTO Apply VALUES (2, 'IIT Bombay', 'Electronics', 'N');
INSERT INTO Apply VALUES (3, 'IIT Indore', 'Computer Science', 'Y');
INSERT INTO Apply VALUES (4, 'IIT Kanpur', 'Mechanical', 'Y');
INSERT INTO Apply VALUES (5, 'IIT Madras', 'Civil', 'N');

COMMIT;


-- ==========================================
-- Q1. NULL VALUES
-- ==========================================

SELECT *
FROM Student
WHERE sName IS NULL;


-- ==========================================
-- Q2. MAXIMUM 10 RECORDS
-- ==========================================

SELECT * FROM Student
FETCH FIRST 10 ROWS ONLY;

SELECT * FROM College
FETCH FIRST 10 ROWS ONLY;

SELECT * FROM Apply
FETCH FIRST 10 ROWS ONLY;


-- ==========================================
-- Q3. LEAST AND GREATEST
-- ==========================================

SELECT SID,
       sName,
       GPA,
       sizeHS,
       LEAST(GPA, sizeHS) AS least_value,
       GREATEST(GPA, sizeHS) AS greatest_value
FROM Student;


SELECT cName,
       enrollment,
       LEAST(enrollment, 10000) AS least_value,
       GREATEST(enrollment, 10000) AS greatest_value
FROM College;


SELECT SID,
       cName,
       major,
       decision,
       LEAST(SID, 10, 20) AS least_value,
       GREATEST(SID, 10, 20) AS greatest_value
FROM Apply;


-- ==========================================
-- Q4. COALESCE
-- ==========================================

SELECT SID,
       sName,
       COALESCE(sName, 'Name Not Available') AS result
FROM Student;


SELECT cName,
       state,
       COALESCE(state, 'State Not Available') AS result
FROM College;


SELECT SID,
       cName,
       decision,
       COALESCE(decision, 'Decision Pending') AS result
FROM Apply;


-- ==========================================
-- EMPLOYEE TABLE
-- ==========================================

CREATE TABLE Employee (
    ID NUMBER PRIMARY KEY,
    Name VARCHAR2(50),
    Email VARCHAR2(100),
    Exp NUMBER,
    Salary NUMBER(10,2),
    Phone VARCHAR2(15),
    Address VARCHAR2(100)
);


INSERT INTO Employee VALUES
(101, 'Aditya', 'aditya@gmail.com', 1, 30000, '9876543210', 'Indore');

INSERT INTO Employee VALUES
(102, 'Rahul', 'rahul@gmail.com', 3, 40000, '9876543211', 'Bhopal');

INSERT INTO Employee VALUES
(103, 'Priya', 'priya@gmail.com', 5, 50000, '9876543212', 'Delhi');

INSERT INTO Employee VALUES
(104, 'Aman', 'aman@gmail.com', 15, 70000, '9876543213', 'Mumbai');

INSERT INTO Employee VALUES
(105, 'Neha', 'neha@gmail.com', 18, 80000, '9876543214', 'Pune');

COMMIT;


-- ==========================================
-- CURSOR (a)
-- 20000 INCREMENT FOR EXP >= 2
-- ==========================================

DECLARE
    CURSOR emp_cursor IS
        SELECT ID
        FROM Employee
        WHERE Exp >= 2;

    v_count NUMBER := 0;

BEGIN

    FOR emp IN emp_cursor
    LOOP

        UPDATE Employee
        SET Salary = Salary + 20000
        WHERE ID = emp.ID;

        v_count := v_count + 1;

    END LOOP;

    COMMIT;

    DBMS_OUTPUT.PUT_LINE(
        'Number of employees affected: ' || v_count
    );

END;
/


-- ==========================================
-- CURSOR (b)
-- INSERT EMPLOYEE
-- ==========================================

INSERT INTO Employee
VALUES (
    106,
    'Vikas',
    'vikas@gmail.com',
    16,
    90000,
    '9876543215',
    'Indore'
);

COMMIT;


-- FETCH EMPLOYEES WITH EXPERIENCE > 14
-- ==========================================

DECLARE

    CURSOR emp_cursor IS
        SELECT ID, Name, Exp, Salary
        FROM Employee
        WHERE Exp > 14;

    v_count NUMBER := 0;

BEGIN

    FOR emp IN emp_cursor
    LOOP

        DBMS_OUTPUT.PUT_LINE(
            'ID: ' || emp.ID ||
            ' Name: ' || emp.Name ||
            ' Experience: ' || emp.Exp ||
            ' Salary: ' || emp.Salary
        );

        v_count := v_count + 1;

    END LOOP;

    DBMS_OUTPUT.PUT_LINE(
        'Number of rows fetched: ' || v_count
    );

END;
/
