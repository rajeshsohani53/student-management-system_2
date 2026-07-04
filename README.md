# Student Management System

A web-based Student Management System built as a team project. The system allows an admin to register students, create courses, and allocate students to course seats — with concurrency control to prevent double-booking.

---

## Tech Stack

| Layer | Technology |
|---|---|
| Language | Java 17 |
| Web Layer | Servlets + JSP |
| Database | MySQL 8 |
| Server | Apache Tomcat 10.1 |
| DB Connectivity | JDBC |

---

## Features

- Register students (name + phone)
- Create courses with seat capacity
- Allocate students to course seats
- Prevent double-booking with concurrency handling
- MVC architecture (Servlets + JSP)

---

## Project Structure

```
student-management-system_2/
├── src/
│   ├── model/          # Entity classes (Student, Course, Seat, Allocation)
│   ├── dao/            # JDBC database access layer
│   ├── service/        # Business logic + concurrency handling
│   └── controller/     # Servlet controllers
├── webapp/
│   ├── WEB-INF/
│   │   └── web.xml
│   └── views/          # JSP pages
└── README.md
```

---

## Team

| Member | Role |
|---|---|
| **Rajesh Sohani** ([@rajeshsohani53](https://github.com/rajeshsohani53)) | Tech Lead — Architecture, Design, Concurrency (Seat & Allocation modules), Code Review |
| **Prajakta Takbhate** ([@prajaktatakbhate2](https://github.com/prajaktatakbhate2)) | Co-Developer — Student & Course modules (full stack), JSP/Frontend Lead, Documentation |

Both members contribute to design, coding, and testing.

---

## Git Workflow

- `main` branch is protected — no direct pushes allowed
- All changes go through Pull Requests
- At least 1 review required before merging
- Branch naming: `feature/your-feature-name`

---

## Setup Instructions

1. Clone the repo
   ```bash
   git clone https://github.com/rajeshsohani53/student-management-system_2.git
   ```
2. Import into Eclipse EE or IntelliJ IDEA as a Maven/Web project
3. Set up MySQL 8 and create the database (schema in `/sql/schema.sql`)
4. Update DB credentials in `src/util/DBConnection.java`
5. Deploy to Apache Tomcat 10.1
6. Access at `http://localhost:8080/student-management-system_2`

---

## Duration

5 days — built from scratch as a learning + portfolio project.
