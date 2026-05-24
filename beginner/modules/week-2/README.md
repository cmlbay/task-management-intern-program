# Week 2 — Entities & Authentication

## Goal
Design the database model and implement JWT-based authentication.
By the end of this week, users will be able to register and log in.

## Topics
- JPA Entity design
- Table relationships (@OneToMany, @ManyToMany, @ManyToOne)
- Spring Security configuration
- JWT token generation and validation

## Tasks

### 1. Create Entities
Create the following entity classes under `src/main/java/.../entity/`:

**User.java**
```java
@Entity
@Table(name = "users")
public class User {
    @Id
    @GeneratedValue(strategy = GenerationType.UUID)
    private UUID id;
    private String username;
    private String email;
    private String password;
    private String fullName;

    @Enumerated(EnumType.STRING)
    private Role role;

    private LocalDateTime createdAt;
    private LocalDateTime updatedAt;
}
```

**Project.java**
```java
@Entity
@Table(name = "projects")
public class Project {
    @Id
    @GeneratedValue(strategy = GenerationType.UUID)
    private UUID id;
    private String name;
    private String description;

    @Enumerated(EnumType.STRING)
    private ProjectStatus status;

    @ManyToOne
    @JoinColumn(name = "owner_id")
    private User owner;

    private LocalDateTime createdAt;
    private LocalDateTime updatedAt;
}
```

**ProjectMember.java**
```java
@Entity
@Table(name = "project_members")
public class ProjectMember {
    @Id
    @GeneratedValue(strategy = GenerationType.UUID)
    private UUID id;

    @ManyToOne
    @JoinColumn(name = "project_id")
    private Project project;

    @ManyToOne
    @JoinColumn(name = "user_id")
    private User user;

    @Enumerated(EnumType.STRING)
    private MemberRole role;

    private LocalDateTime joinedAt;
}
```

**Task.java**
```java
@Entity
@Table(name = "tasks")
public class Task {
    @Id
    @GeneratedValue(strategy = GenerationType.UUID)
    private UUID id;
    private String title;
    private String description;

    @Enumerated(EnumType.STRING)
    private TaskStatus status;

    @Enumerated(EnumType.STRING)
    private TaskPriority priority;

    private LocalDate dueDate;

    @ManyToOne
    @JoinColumn(name = "project_id")
    private Project project;

    @ManyToOne
    @JoinColumn(name = "assignee_id")
    private User assignee;

    @ManyToOne
    @JoinColumn(name = "reporter_id")
    private User reporter;

    private LocalDateTime createdAt;
    private LocalDateTime updatedAt;
}
```

### 2. Create Enums
Create the following enums under `src/main/java/.../enums/`:

```java
public enum Role { ADMIN, MEMBER }
public enum ProjectStatus { ACTIVE, ARCHIVED }
public enum MemberRole { MANAGER, MEMBER }
public enum TaskStatus { TODO, IN_PROGRESS, IN_REVIEW, DONE }
public enum TaskPriority { LOW, MEDIUM, HIGH, CRITICAL }
```

### 3. Implement JWT Authentication

**Step 1 — Add JWT dependency to pom.xml:**
```xml
<dependency>
    <groupId>io.jsonwebtoken</groupId>
    <artifactId>jjwt-api</artifactId>
    <version>0.11.5</version>
</dependency>
```

**Step 2 — Create JwtService:**
- Generate token from user details
- Extract username from token
- Validate token

**Step 3 — Configure SecurityFilterChain:**
- Permit `/api/v1/auth/**` endpoints
- Require authentication for all other endpoints
- Add JWT filter to the chain

### 4. Implement Auth Endpoints

| Method | Endpoint | Description |
|---|---|---|
| POST | `/api/v1/auth/register` | Create new account |
| POST | `/api/v1/auth/login` | Get JWT token |
| GET | `/api/v1/auth/me` | Get current user info |

### 5. Test with Postman
- Register a new user
- Login and copy the JWT token
- Call `/auth/me` with the token in the Authorization header:
  `Bearer your_token_here`

## Deliverables
- [ ] All entities created and tables generated in database
- [ ] Enums defined
- [ ] Register endpoint works
- [ ] Login endpoint returns JWT token
- [ ] `/auth/me` returns current user info
- [ ] Password is hashed (BCrypt), never stored as plain text
- [ ] Password is NOT returned in any response
- [ ] PR opened with branch name `feature/week-2-auth`

## Resources
- [Spring Security Documentation](https://docs.spring.io/spring-security/reference/)
- [JWT Introduction](https://jwt.io/introduction)
- [Baeldung — Spring Security JWT](https://www.baeldung.com/spring-security-oauth-jwt)
