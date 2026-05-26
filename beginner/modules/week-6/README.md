# Week 6 — Swagger & Docker (Optional)

## Goal
Document the API with Swagger UI and containerize the application with Docker.
By the end of this week, your project will be fully documented and
ready to run in any environment with a single command.

## Topics
- API documentation with SpringDoc OpenAPI
- Swagger UI
- Docker basics
- Docker Compose

## Tasks

### 1. Add Swagger Documentation

Add SpringDoc OpenAPI dependency to `pom.xml`:

````xml
<dependency>
    <groupId>org.springdoc</groupId>
    <artifactId>springdoc-openapi-starter-webmvc-ui</artifactId>
    <version>2.3.0</version>
</dependency>
````

Add the following to `application.yml`:

````yaml
springdoc:
  api-docs:
    path: /api-docs
  swagger-ui:
    path: /swagger-ui.html
````

### 2. Document Your Endpoints

Add annotations to your controllers:

````java
@Tag(name = "Task Management", description = "Task CRUD operations")
@RestController
@RequestMapping("/api/v1/projects/{projectId}/tasks")
public class TaskController {

    @Operation(summary = "Create a new task")
    @ApiResponses(value = {
        @ApiResponse(responseCode = "201", description = "Task created successfully"),
        @ApiResponse(responseCode = "403", description = "User is not a project member"),
        @ApiResponse(responseCode = "404", description = "Project not found")
    })
    @PostMapping
    public ResponseEntity<TaskResponse> createTask(...) { }
}
````

Document all controllers:
- `AuthController`
- `ProjectController`
- `TaskController`

### 3. Configure Swagger for JWT

Add JWT support to Swagger UI so you can test authenticated endpoints:

````java
@Configuration
public class SwaggerConfig {

    @Bean
    public OpenAPI openAPI() {
        return new OpenAPI()
            .info(new Info()
                .title("Task Management API")
                .description("Internship project — Task Management REST API")
                .version("1.0.0"))
            .addSecurityItem(new SecurityRequirement().addList("Bearer Authentication"))
            .components(new Components()
                .addSecuritySchemes("Bearer Authentication",
                    new SecurityScheme()
                        .type(SecurityScheme.Type.HTTP)
                        .scheme("bearer")
                        .bearerFormat("JWT")));
    }
}
````

### 4. Create Dockerfile

Create `Dockerfile` in the root of your project:

````dockerfile
FROM eclipse-temurin:17-jdk-alpine
WORKDIR /app
COPY target/*.jar app.jar
ENTRYPOINT ["java", "-jar", "app.jar"]
````

### 5. Create docker-compose.yml

Create `docker-compose.yml` in the root of your project:

````yaml
version: '3.8'

services:
  app:
    build: .
    ports:
      - "8080:8080"
    environment:
      SPRING_DATASOURCE_URL: jdbc:postgresql://db:5432/taskmanagement
      SPRING_DATASOURCE_USERNAME: postgres
      SPRING_DATASOURCE_PASSWORD: postgres
    depends_on:
      - db

  db:
    image: postgres:15-alpine
    environment:
      POSTGRES_DB: taskmanagement
      POSTGRES_USER: postgres
      POSTGRES_PASSWORD: postgres
    ports:
      - "5432:5432"
    volumes:
      - postgres_data:/var/lib/postgresql/data

volumes:
  postgres_data:
````

### 6. Build and Run with Docker

````bash
# Build the jar
mvn clean package -DskipTests

# Build and start containers
docker-compose up --build

# Stop containers
docker-compose down
````

### 7. Update README

Add Docker setup instructions to your project README:

````markdown
## Running with Docker

### Prerequisites
- Docker
- Docker Compose

### Steps
1. Clone the repository
2. Run the following command:
```bash
   docker-compose up --build
```
3. API will be available at `http://localhost:8080`
4. Swagger UI will be available at `http://localhost:8080/swagger-ui.html`
````

## Deliverables
- [ ] SpringDoc OpenAPI dependency added
- [ ] Swagger UI accessible at `/swagger-ui.html`
- [ ] All endpoints documented in Swagger
- [ ] JWT authentication works in Swagger UI
- [ ] Dockerfile created
- [ ] docker-compose.yml created
- [ ] Application runs successfully with `docker-compose up --build`
- [ ] README updated with Docker setup instructions
- [ ] PR opened with branch name `feature/week-6-swagger-docker`

## Resources
- [SpringDoc OpenAPI](https://springdoc.org/)
- [Docker Documentation](https://docs.docker.com/)
- [Docker Compose Documentation](https://docs.docker.com/compose/)
- [eclipse-temurin Docker Image](https://hub.docker.com/_/eclipse-temurin)
