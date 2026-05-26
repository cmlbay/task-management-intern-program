# Application Architecture

## Layer Architecture

```mermaid
graph TD
    Client(["Client (Postman / Frontend)"])

    subgraph Spring Boot Application
        SC["Security Filter Chain (JWT)"]
        CT["Controller Layer"]
        SV["Service Layer"]
        RP["Repository Layer"]
    end

    DB[("PostgreSQL Database")]

    Client -->|HTTP Request| SC
    SC -->|Authenticated Request| CT
    CT -->|Business Logic| SV
    SV -->|Data Access| RP
    RP -->|SQL| DB
    DB -->|Result| RP
    RP -->|Entity| SV
    SV -->|DTO| CT
    CT -->|HTTP Response| Client
```

## Layer Responsibilities

| Layer | Class | Responsibility |
|---|---|---|
| Controller | `*Controller.java` | Receives HTTP requests, returns HTTP responses |
| Service | `*Service.java` | Contains business logic and authorization rules |
| Repository | `*Repository.java` | Database operations via JPA |
| Entity | `*.java` | Database table mapping |
| DTO | `*Request.java` / `*Response.java` | Data transfer between layers |

## Security Flow

```mermaid
sequenceDiagram
    participant Client
    participant JwtFilter
    participant Controller
    participant Service

    Client->>JwtFilter: HTTP Request + Bearer Token
    JwtFilter->>JwtFilter: Validate Token
    alt Token Valid
        JwtFilter->>Controller: Forward Request
        Controller->>Service: Process
        Service->>Controller: Result
        Controller->>Client: 200 OK
    else Token Invalid
        JwtFilter->>Client: 401 Unauthorized
    end
```

## Package Structure

```
src/main/java/com/example/taskmanagement/
├── controller/
│   ├── AuthController.java
│   ├── ProjectController.java
│   └── TaskController.java
├── service/
│   ├── AuthService.java
│   ├── ProjectService.java
│   └── TaskService.java
├── repository/
│   ├── UserRepository.java
│   ├── ProjectRepository.java
│   ├── ProjectMemberRepository.java
│   └── TaskRepository.java
├── entity/
│   ├── User.java
│   ├── Project.java
│   ├── ProjectMember.java
│   └── Task.java
├── dto/
│   ├── request/
│   │   ├── RegisterRequest.java
│   │   ├── LoginRequest.java
│   │   ├── CreateProjectRequest.java
│   │   └── CreateTaskRequest.java
│   └── response/
│       ├── AuthResponse.java
│       ├── ProjectResponse.java
│       └── TaskResponse.java
├── enums/
│   ├── Role.java
│   ├── ProjectStatus.java
│   ├── MemberRole.java
│   ├── TaskStatus.java
│   └── TaskPriority.java
├── exception/
│   ├── GlobalExceptionHandler.java
│   ├── ResourceNotFoundException.java
│   └── ForbiddenException.java
└── security/
    ├── JwtService.java
    ├── JwtAuthFilter.java
    └── SecurityConfig.java
```
