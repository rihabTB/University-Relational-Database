# Checkpoint: University Information System (Relational Database)

## Step 1 – Schema Design

**Entities & Attributes:**

- **Students**
  - student_id (INT, PRIMARY KEY)
  - name (VARCHAR, NOT NULL)
  - email (VARCHAR, UNIQUE, NOT NULL)
  - age (INT, CHECK (age > 17), NOT NULL)

- **Instructors**
  - instructor_id (INT, PRIMARY KEY)
  - name (VARCHAR, NOT NULL)
  - department (VARCHAR, NOT NULL)

- **Courses**
  - course_id (INT, PRIMARY KEY)
  - title (VARCHAR, NOT NULL)
  - credits (INT, NOT NULL)
  - instructor_id (INT, FOREIGN KEY REFERENCES Instructors(instructor_id))

- **Enrollments**
  - enrollment_id (INT, PRIMARY KEY)
  - student_id (INT, FOREIGN KEY REFERENCES Students(student_id))
  - course_id (INT, FOREIGN KEY REFERENCES Courses(course_id))
  - grade (CHAR(2))

**Normalization:**
- All tables are in **3NF**:
  - No repeating groups → 1NF
  - Non-key attributes depend on the whole primary key → 2NF
  - No transitive dependencies → 3NF

---

## Step 2 – SQL Table Creation

```sql
CREATE TABLE Students (
    student_id INT PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    email VARCHAR(100) UNIQUE NOT NULL,
    age INT NOT NULL CHECK (age > 17)
);

CREATE TABLE Instructors (
    instructor_id INT PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    department VARCHAR(100) NOT NULL
);

CREATE TABLE Courses (
    course_id INT PRIMARY KEY,
    title VARCHAR(100) NOT NULL,
    credits INT NOT NULL,
    instructor_id INT,
    FOREIGN KEY (instructor_id) REFERENCES Instructors(instructor_id)
);

CREATE TABLE Enrollments (
    enrollment_id INT PRIMARY KEY,
    student_id INT,
    course_id INT,
    grade CHAR(2),
    FOREIGN KEY (student_id) REFERENCES Students(student_id),
    FOREIGN KEY (course_id) REFERENCES Courses(course_id)
);
```
## Step 3 – Insert Sample Data

```sql
-- Insert Students 
INSERT INTO Students (student_id, name, email, age)
VALUES 
(1, 'Alice Smith', 'alice@example.com', 20),
(2, 'Bob Johnson', 'bob@example.com', 22),
(3, 'Carol Lee', 'carol@example.com', 19);

-- Insert Instructors
INSERT INTO Instructors (instructor_id, name, department)
VALUES
(1, 'Dr. Adams', 'Computer Science'),
(2, 'Dr. Brown', 'Mathematics'),
(3, 'Dr. Clark', 'Physics');

-- Insert Courses
INSERT INTO Courses (course_id, title, credits, instructor_id)
VALUES
(1, 'Database Systems', 3, 1),
(2, 'Calculus I', 4, 2),
(3, 'Physics 101', 3, 3);

-- Insert Enrollments
INSERT INTO Enrollments (enrollment_id, student_id, course_id, grade)
VALUES
(1, 1, 1, 'A'),
(2, 1, 2, 'B'),
(3, 2, 1, 'C'),
(4, 3, 3, 'B');

```

## Step 4 – Query Execution

```sql
-- 1. Retrieve all students enrolled in "Database Systems"
SELECT s.name, s.email
FROM Students s
JOIN Enrollments e ON s.student_id = e.student_id
JOIN Courses c ON e.course_id = c.course_id
WHERE c.title = 'Database Systems';

-- 2. List all courses along with the names of their instructors
SELECT c.title, i.name AS instructor_name
FROM Courses c
JOIN Instructors i ON c.instructor_id = i.instructor_id;

-- 3. Find students who are not enrolled in any course
SELECT s.name, s.email
FROM Students s
LEFT JOIN Enrollments e ON s.student_id = e.student_id
WHERE e.enrollment_id IS NULL;

-- 4. Update the email address of a student
UPDATE Students
SET email = 'alice.new@example.com'
WHERE student_id = 1;

-- 5. Delete a course by its ID
DELETE FROM Courses
WHERE course_id = 3;
```
