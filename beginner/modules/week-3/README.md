# Week 3 — Project & Member Management

## Goal
Implement Project CRUD operations and member management.
By the end of this week, users will be able to create projects,
invite members, and manage project roles with proper authorization.

## Topics
- Service layer responsibilities
- Authorization logic
- Exception handling
- Bean Validation

## Tasks

### 1. Create Project CRUD

Create the following classes:
- `ProjectController.java`
- `ProjectService.java`
- `ProjectRepository.java`

Implement these endpoints:

| Method | Endpoint | Description | Who Can Access |
|---|---|---|---|
| POST | `/api/v1/projects` | Create project | Any authenticated user |
| GET | `/api/v1/projects` | List my projects | Any authenticated user |
| GET | `/api/v1/projects/{id}` | Get project detail | Project members only |
| PUT | `/api/v1/projects/{id}` | Update project | Owner only |
| DELETE | `/api/v1/projects/{id}` | Archive project | Owner only |

**Important:** DELETE does NOT remove the record from database.
It changes the project status to `ARCHIVED`. This is called soft delete.

### 2. Create Member Management Endpoints

| Method | Endpoint | Description | Who Can Access |
|---|---|---|---|
| POST | `/api/v1/projects/{id}/members` | Add member | Owner or Manager |
| DELETE | `/api/v1/projects/{id}/members/{userId}` | Remove member | Owner or Manager |
| GET | `/api/v1/projects/{id}/members` | List members | Project members only |

### 3. Implement Authorization Rules

Add authorization checks in your service layer:

```java
// Example: Only owner can update project
public ProjectResponse updateProject(UUID projectId, UpdateProjectRequest request, User currentUser) {
    Project project = projectRepository.findById(projectId)
        .orElseThrow(() -> new ResourceNotFoundException("Project not found"));

    if (!project.getOwner().getId().equals(currentUser.getId())) {
        throw new ForbiddenException("Only the project owner can update this project");
    }

    // update logic here
}
```

Authorization rules summary:
- Any authenticated user can create a project
- Only project members can view project details
- Only owner can update or archive a project
- Only owner or manager can add/remove members

### 4. Implement Global Exception Handler

Create `GlobalExceptionHandler.java` with `@ControllerAdvice`:

```java
@ControllerAdvice
public class GlobalExceptionHandler {

    @ExceptionHandler(ResourceNotFoundException.class)
    public ResponseEntity<ErrorResponse> handleNotFound(ResourceNotFoundException ex) {
        return ResponseEntity.status(404).body(
            new ErrorResponse(404, "Not Found", ex.getMessage())
        );
    }

    @ExceptionHandler(ForbiddenException.class)
    public ResponseEntity<ErrorResponse> handleForbidden(ForbiddenException ex) {
        return ResponseEntity.status(403).body(
            new ErrorResponse(403, "Forbidden", ex.getMessage())
        );
    }

    @ExceptionHandler(MethodArgumentNotValidException.class)
    public ResponseEntity<ErrorResponse> handleValidation(MethodArgumentNotValidException ex) {
        String message = ex.getBindingResult().getFieldErrors().stream()
            .map(FieldError::getDefaultMessage)
            .findFirst()
            .orElse("Validation failed");
        return ResponseEntity.status(400).body(
            new ErrorResponse(400, "Bad Request", message)
        );
    }
}
```

### 5. Add Bean Validation

Add validation annotations to your request DTOs:

```java
public class CreateProjectRequest {
    @NotBlank(message = "Project name cannot be blank")
    @Size(min = 3, max = 100, message = "Project name must be between 3 and 100 characters")
    private String name;

    @Size(max = 500, message = "Description cannot exceed 500 characters")
    private String description;
}
```

### 6. Standard Error Response

All error responses must follow this format:

```json
{
  "timestamp": "2024-01-15T10:00:00Z",
  "status": 403,
  "error": "Forbidden",
  "message": "Only the project owner can update this project",
  "path": "/api/v1/projects/uuid"
}
```

### 7. Test with Postman

Test the following scenarios:
- Create a project and verify you are the owner
- Try to update a project with a different user → expect 403
- Add a member to your project
- View project details with a member account
- Archive a project and verify it is not deleted from database

## Deliverables
- [ ] Project CRUD endpoints working
- [ ] Soft delete implemented (status → ARCHIVED)
- [ ] Member add/remove endpoints working
- [ ] Authorization rules enforced
- [ ] Unauthorized access returns 403
- [ ] Validation errors return 400
- [ ] Global exception handler active
- [ ] Error responses follow standard format
- [ ] PR opened with branch name `feature/week-3-project-management`

## Resources
- [Spring Validation](https://docs.spring.io/spring-framework/docs/current/reference/html/core.html#validation)
- [Baeldung — Exception Handling in Spring](https://www.baeldung.com/exception-handling-for-rest-with-spring)
- [REST API Best Practices](https://restfulapi.net/)
