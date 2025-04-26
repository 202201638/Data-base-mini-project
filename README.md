📚 Mini Library Management System & Relational Algebra Optimization 

🔍 Project Overview

This project showcases two database-focused tasks:

Task 1: Design and implementation of a Mini Library Management System using SQL (with ERD, procedures, functions, triggers, and indexes).
Task 2: Relational Algebra translation and query optimization using heuristic techniques.

📘 Task 1: Mini Library Management System

🎯 Objective

To design a mini-library management system and implement core operations using:

ERD design principles

Stored Procedures

Scalar Functions

Triggers

Indexing for performance

📐 ERD Design
Entities:
Books(BookID, Title, Author, CopiesAvailable, TotalCopies)
Members(MemberID, Name, Email, TotalBooksBorrowed, IsActive)
BorrowedBooks(BorrowID, MemberID, BookID, BorrowDate, DueDate, ReturnDate, IsReturned)

Relationships:
A Member can borrow many books.
A Book can appear in many borrow records.

Includes:
Cardinality & Participation Constraints
Avoidance of fan traps and chasm traps
🧩 SQL Implementation
📋 Table Creation

Includes CREATE TABLE statements for all three entities with appropriate constraints.
⚙️ Stored Procedure: BorrowBook

Implements logic to:
Check member status
Check book availability
Insert into BorrowedBooks
Update CopiesAvailable and TotalBooksBorrowed

📌 Indexing
Indexes created on:
Books(BookID)
Members(MemberID)

📌 These fields are commonly used for joins and lookups, so indexing improves performance.

🧮 Function: GetBooksBorrowed(MemberID)
Returns the number of books a member currently has (not yet returned).

🔐 Trigger: PreventBorrowIfNoCopies
Prevents inserts into BorrowedBooks if:
CopiesAvailable = 0
OR IsActive = false

🧪 Sample Data

Includes sample insertions for:
3 books
3 members
5 borrowing attempts (some successful, some blocked by logic)

📘 Task 2: SQL to Relational Algebra Conversion

🧾 Original SQL Query
SELECT S.Name, C.Title, E.Grade
FROM STUDENT S, COURSE C, ENROLLMENT E
WHERE S.StudentID = E.StudentID
  AND C.CourseID = E.CourseID
  AND S.Major = 'Computer Science'
  AND C.Credits >= 3;

🛠️ Initial Relational Algebra Expression (Unoptimized)
π_{S.Name, C.Title, E.Grade} (
    σ_{S.StudentID = E.StudentID AND C.CourseID = E.CourseID AND S.Major = 'Computer Science' AND C.Credits >= 3}
    (STUDENT × COURSE × ENROLLMENT)
)

🌲 Query Tree Representation
π_{S.Name, C.Title, E.Grade}
         |
σ_{conditions}
         |
STUDENT × COURSE × ENROLLMENT

✅ Optimized Relational Algebra Expression
π_{S.Name, C.Title, E.Grade} (
  (σ_{S.Major = 'Computer Science'}(STUDENT)
   ⋈_{S.StudentID = E.StudentID} ENROLLMENT)
   ⋈_{C.CourseID = E.CourseID}
   σ_{C.Credits >= 3}(COURSE)
)

📝 Optimization Steps
Selection Pushdown: Apply filters (S.Major, C.Credits) early.
Join Reordering: Replace Cartesian product with theta joins.
Projection Late: Project only necessary columns at the end.



