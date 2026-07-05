# Student Management System

![Java](https://img.shields.io/badge/Java-17-orange)
![MySQL](https://img.shields.io/badge/MySQL-8.0-blue)
![Tomcat](https://img.shields.io/badge/Tomcat-10.1-yellow)
![Status](https://img.shields.io/badge/Status-In%20Development-yellow)
![License](https://img.shields.io/badge/License-MIT-green)

A web-based Student Management System built as a team project. The system allows an admin to register students, create courses, and allocate students to course seats — with **three-layer concurrency control** to prevent double-booking under simultaneous requests.

> **Duration:** 5 days · **Type:** Learning + Portfolio project · **Team size:** 2

---

## Features

- Register students (name + phone)
- Create courses with configurable seat capacity
- Allocate students to specific seats in a course
- Prevent double-booking with layered concurrency handling
- Soft-delete support for auditability
- REST API following standard HTTP conventions
- JSP-based admin interface

---

## Tech Stack

| Layer           | Technology         |
| --------------- | ------------------ |
| Language        | Java 17            |
| Web Layer       | Servlets + JSP     |
| Database        | MySQL 8            |
| Server          | Apache Tomcat 10.1 |
| DB Connectivity | JDBC + HikariCP    |
| Build Tool      | Maven              |
| Testing         | Postman            |

---

## Architecture

The project follows a **layered architecture** with strict downward dependency — no upward imports allowed.

```
Client (Browser / Postman)
        ↓
Controller Layer  (Servlets)         → HTTP handling only
        ↓
Service Layer                        → Business logic + transactions
        ↓
DAO Layer         (JDBC)             → SQL queries
        ↓
Model Layer       (POJOs)            → Student, Course, Seat, Allocation
        ↓
MySQL Database
```

**Key principle:** Controller depends on Service. Service depends on DAO. DAO depends on Model. Nothing points upward.

> See [DESIGN.md](./DESIGN.md) for full system design, schema, and API contracts.

---

## Concurrency Handling

The seat allocation flow uses **three-layer defense** to prevent double-booking under concurrent requests. This is the technical highlight of the project.

### Layer 1 — Database Unique Constraint
A partial unique index on `allocations(seat_id) WHERE status='ACTIVE'` ensures the database itself refuses to insert a duplicate active allocation for the same seat.

### Layer 2 — Pessimistic Locking (main defense)
Inside `AllocationService.allocate()`, the seat row is locked before any write:
```sql
BEGIN TRANSACTION;
SELECT * FROM seats WHERE seat_id = ? FOR UPDATE;
-- verify availability
INSERT INTO allocations (...);
COMMIT;
```
Only one thread holds the lock at a time; others wait.

### Layer 3 — Optimistic Locking (for updates/cancellations)
A `version` column on `allocations` guards concurrent updates:
```sql
UPDATE allocations
SET status='CANCELLED', version = version + 1
WHERE allocation_id = ? AND version = ?;
```
If the version has changed since it was read, the update fails safely.

**Target:** Handle up to 50 simultaneous booking attempts with zero double-bookings.

---

## Project Structure

```
student-management-system_2/
├── src/
│   └── main/
│       ├── java/com/sms/
│       │   ├── model/          # POJOs — Student, Course, Seat, Allocation
│       │   ├── dao/            # JDBC access layer
│       │   ├── service/        # Business logic + concurrency handling
│       │   ├── controller/     # Servlet controllers
│       │   ├── dto/            # Request / Response DTOs
│       │   ├── exception/      # Custom exceptions
│       │   └── util/           # DBConnection, Validator
│       └── webapp/
│           ├── WEB-INF/
│           │   └── web.xml
│           └── views/          # JSP pages
├── sql/
│   └── schema.sql              # Database schema
├── DESIGN.md                   # System design document
└── README.md
```

---

## API Endpoints

| Method | Endpoint                              | Description                    |
| ------ | ------------------------------------- | ------------------------------ |
| POST   | `/api/v1/students`                    | Register a student             |
| GET    | `/api/v1/students/{id}`               | Get student details            |
| GET    | `/api/v1/students/{id}/allocations`   | View student's allocations     |
| POST   | `/api/v1/courses`                     | Create a course with N seats   |
| GET    | `/api/v1/courses`                     | List all courses               |
| GET    | `/api/v1/courses/{id}/seats`          | View seats in a course         |
| POST   | `/api/v1/allocations`                 | Allocate a seat (concurrency)  |
| DELETE | `/api/v1/allocations/{id}`            | Cancel an allocation           |

---

## Prerequisites

Before setting up the project, make sure you have:

- Java 17 or higher installed (`java -version`)
- MySQL 8.0+ running locally
- Apache Tomcat 10.1
- Maven 3.8+
- An IDE (Eclipse EE / IntelliJ IDEA / VS Code)
- Postman (for API testing)

---

## Setup Instructions

### 1. Clone the repository
```bash
git clone https://github.com/rajeshsohani53/student-management-system_2.git
cd student-management-system_2
```

### 2. Set up the database
```bash
mysql -u root -p < sql/schema.sql
```

### 3. Configure database credentials
Update your MySQL username and password in:
```
src/main/java/com/sms/util/DBConnection.java
```

### 4. Import into your IDE
Import as a Maven Web Application project.

### 5. Deploy to Tomcat
Build the WAR file and deploy to Apache Tomcat 10.1.

### 6. Access the application
```
http://localhost:8080/student-management-system_2
```

---

## Team

| Member                                                                             | Role                                                                                   |
| ---------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------- |
| **Rajesh Sohani** ([@rajeshsohani53](https://github.com/rajeshsohani53))           | Tech Lead — Architecture, Design, Concurrency (Seat & Allocation modules), Code Review |
| **Prajakta Takbhate** ([@prajaktatakbhate2](https://github.com/prajaktatakbhate2)) | Co-Developer — Student & Course modules (full stack), JSP/Frontend Lead, Documentation |

Both members contribute to design, coding, and testing.

---

## Git Workflow

- `main` branch is protected — no direct pushes
- All changes go through Pull Requests
- At least **1 review** required before merging
- Branch naming: `feature/your-feature-name`
- Commit style: [Conventional Commits](https://www.conventionalcommits.org/)
  - `feat: add student registration endpoint`
  - `fix: correct seat allocation lock order`
  - `docs: update README`

---

## Documentation

- [DESIGN.md](./DESIGN.md) — System design, schema, architecture decisions
- [API.md](./API.md) — Full REST API reference *(coming soon)*
- [SETUP.md](./SETUP.md) — Detailed setup guide *(coming soon)*

---

## License

This project is licensed under the MIT License — see the [LICENSE](./LICENSE) file for details.

---

## Acknowledgments

Built as part of the Java Full Stack learning journey at **Cyber Success, Pune** — Campus Connect Internship Program.
