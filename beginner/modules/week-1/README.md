# Week 1 — Setup & Git Flow

## Goal
Set up the development environment, create the Spring Boot project,
and learn the Git workflow you will follow throughout the program.

## Topics
- Spring Boot project structure
- Maven dependencies
- Git branching strategy
- PostgreSQL connection

## Tasks

### 1. Review Java & OOP Concepts
Before starting, make sure you are comfortable with:
- Classes, interfaces, inheritance
- Access modifiers (public, private, protected)
- Collections (List, Map)

### 2. Create Spring Boot Project
- Go to [start.spring.io](https://start.spring.io)
- Configure your project:
  - **Project:** Maven
  - **Language:** Java
  - **Spring Boot:** 4.x
  - **Java:** 17+
- Add the following dependencies:
  - Spring Web
  - Spring Data JPA
  - Spring Security
  - PostgreSQL Driver
  - Lombok

### 3. Configure Database
Create `src/main/resources/application.yml` and add:

```yaml
spring:
  datasource:
    url: jdbc:postgresql://localhost:5432/taskmanagement
    username: your_db_username
    password: your_db_password
  jpa:
    hibernate:
      ddl-auto: update
    show-sql: true
```

### 4. Learn Git Flow
You will follow this branching strategy throughout the program:

| Branch | Purpose |
|---|---|
| `main` | Protected, stable code only |
| `develop` | Integration branch |
| `feature/xxx` | Your weekly work |

**Week 1 branch name:** `feature/week-1-setup`

### 5. Write Hello World Endpoint
Create a simple endpoint to verify your setup:
- `GET /api/v1/hello` → returns `"Hello, World!"`

### 6. Open Your First Pull Request
- Push your `feature/week-1-setup` branch
- Open a PR to `main`
- Make sure the PR checklist is complete

## Deliverables
- [ ] Spring Boot project runs without errors
- [ ] Database connection is established
- [ ] `GET /api/v1/hello` returns a response
- [ ] `application.yml` is configured (do NOT commit real passwords)
- [ ] First PR is opened

## Resources
- [Spring Boot Documentation](https://docs.spring.io/spring-boot/docs/current/reference/html/)
- [Git Branching](https://git-scm.com/book/en/v2/Git-Branching-Basic-Branching-and-Merging)
- [PostgreSQL Download](https://www.postgresql.org/download/)

## Notes
- Never commit your database password to GitHub
- Add `application.yml` to `.gitignore` and share credentials separately
- Ask for help if you are stuck for more than 30 minutes on the same issue
