# Week 4 — Task Management & Final Delivery

## Goal
Implement Task CRUD operations and deliver a fully working API.
By the end of this week, the entire system will be functional end-to-end.

## Topics
- Task management operations
- Filtering and pagination
- End-to-end testing with Postman
- API documentation in README

## Tasks

### 1. Create Task CRUD

Create the following classes:
- `TaskController.java`
- `TaskService.java`
- `TaskRepository.java`

Implement these endpoints:

| Method | Endpoint | Description | Who Can Access |
|---|---|---|---|
| POST | `/api/v1/projects/{projectId}/tasks` | Create task | Project members only |
| GET | `/api/v1/projects/{projectId}/tasks` | List tasks | Project members only |
| GET | `/api/v1/projects/{projectId}/tasks/{id}` | Get task detail | Project members only |
| PUT | `/api/v1/projects/{projectId}/tasks/{id}` | Update task | Reporter or Assignee |
| DELETE | `/api/v1/projects/{projectId}/tasks/{id}` | Delete task | Reporter only |
| PATCH | `/api/v1/projects/{projectId}/tasks/{id}/status` | Update status | Project members only |
| PATCH | `/api/v1/projects/{projectId}/tasks/{id}/assignee` | Update assignee | Project members only |

### 2. Implement Task Filtering

The `GET /projects/{projectId}/tasks` endpoint must support filtering:

```java
@GetMapping
public ResponseEntity<List<TaskResponse>> getTasks(
    @PathVariable UUID projectId,
    @RequestParam(required = false) TaskStatus status,
    @RequestParam(required = false) TaskPriority priority,
    @RequestParam(required = false) UUID assigneeId
) {
    return ResponseEntity.ok(taskService.getTasks(projectId, status, priority, assigneeId));
}
```

Example filter queries:
- `?status=IN_PROGRESS`
- `?priority=HIGH`
- `?assigneeId=uuid`
- `?status=TODO&priority=CRITICAL`

### 3. Implement Task Status Flow

Task status must follow this flow: TODO → IN_PROGRESS → IN_REVIEW → DONE

Add validation in your service layer. A task cannot skip steps.
For example, a task cannot go from `TODO` directly to `DONE`.

### 4. End-to-End Postman Test

Test the complete flow from start to finish:

1. Register two users (User A and User B)
2. User A logs in and creates a project
3. User A adds User B as a member
4. User A creates a task and assigns it to User B
5. User B logs in and views the task
6. User B updates task status to `IN_PROGRESS`
7. User B updates task status to `IN_REVIEW`
8. User A updates task status to `DONE`
9. User A archives the project

### 5. Complete Your Project README

Your project repository must have a README with the following sections:

- Project description
- Tech stack
- Prerequisites
- Setup and run instructions
- API endpoints list
- Postman collection import instructions

### 6. Final PR Checklist

Before opening your final PR, verify everything works:

- All endpoints tested in Postman
- No hardcoded credentials in code
- Error handling works for all edge cases
- README is complete and accurate
- Postman collection is exported and added to repository

## Deliverables
- [ ] All Task endpoints working
- [ ] Task filtering working (status, priority, assignee)
- [ ] Task status flow validated (no skipping steps)
- [ ] Authorization rules enforced
- [ ] End-to-end Postman test completed successfully
- [ ] Project README complete with setup instructions
- [ ] Postman collection exported and committed
- [ ] Final PR opened with branch name `feature/week-4-task-management`

## Congratulations 🎉
You have built a fully functional Task Management REST API from scratch.
You now have a real-world backend project in your portfolio.

### What You Have Learned
- Spring Boot project setup and configuration
- JWT-based authentication and authorization
- RESTful API design principles
- JPA entity relationships
- Service layer architecture
- Exception handling and validation
- Git workflow and code review process

### Next Steps (Optional)
- [Week 5 — Testing](../week-5/README.md)
- [Week 6 — Swagger & Docker](../week-6/README.md)
