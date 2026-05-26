# Task Management API — Project Definition

## Overview
You will build a RESTful Task Management API using Java and Spring Boot.
This project simulates a real-world backend system where teams can manage
their projects and tasks collaboratively.

## Business Requirements

### User Management
- Users can register and log in to the system
- Each user has a system-wide role: ADMIN or MEMBER
- Users can view their own profile

### Project Management
- Authenticated users can create projects
- Project creator automatically becomes the owner
- Owner can update or archive a project
- Owner or Manager can add and remove members
- Each member has a project-level role: MANAGER or MEMBER
- Only project members can view project details

### Task Management
- Project members can create tasks within a project
- Each task has a title, description, priority, due date, and status
- Tasks can be assigned to project members
- Task status follows a strict flow: TODO → IN_PROGRESS → IN_REVIEW → DONE
- Only the reporter or assignee can update a task
- Only the reporter can delete a task
- Tasks can be filtered by status, priority, and assignee

## Domain Model

### Entities

**User**
- id, username, email, password, fullName, role, createdAt, updatedAt

**Project**
- id, name, description, status, owner, createdAt, updatedAt

**ProjectMember**
- id, project, user, role, joinedAt

**Task**
- id, title, description, status, priority, dueDate, project, assignee, reporter, createdAt, updatedAt

### Entity Relationship
User ────────────── ProjectMember ────────── Project
│                                               │
│ (reporter / assignee)                         │
└──────────────────── Task ─────────────────────┘

## Technical Requirements

- Language: Java 17
- Framework: Spring Boot 3.x
- Security: Spring Security + JWT
- Database: PostgreSQL
- ORM: Spring Data JPA / Hibernate
- Build Tool: Maven
- Code Style: Lombok for boilerplate reduction

## API Summary

### Auth
| Method | Endpoint | Description |
|---|---|---|
| POST | `/api/v1/auth/register` | Register |
| POST | `/api/v1/auth/login` | Login, get JWT |
| GET | `/api/v1/auth/me` | Current user info |

### Projects
| Method | Endpoint | Description |
|---|---|---|
| POST | `/api/v1/projects` | Create project |
| GET | `/api/v1/projects` | List my projects |
| GET | `/api/v1/projects/{id}` | Project detail |
| PUT | `/api/v1/projects/{id}` | Update project |
| DELETE | `/api/v1/projects/{id}` | Archive project |
| POST | `/api/v1/projects/{id}/members` | Add member |
| DELETE | `/api/v1/projects/{id}/members/{userId}` | Remove member |
| GET | `/api/v1/projects/{id}/members` | List members |

### Tasks
| Method | Endpoint | Description |
|---|---|---|
| POST | `/api/v1/projects/{projectId}/tasks` | Create task |
| GET | `/api/v1/projects/{projectId}/tasks` | List tasks |
| GET | `/api/v1/projects/{projectId}/tasks/{id}` | Task detail |
| PUT | `/api/v1/projects/{projectId}/tasks/{id}` | Update task |
| DELETE | `/api/v1/projects/{projectId}/tasks/{id}` | Delete task |
| PATCH | `/api/v1/projects/{projectId}/tasks/{id}/status` | Update status |
| PATCH | `/api/v1/projects/{projectId}/tasks/{id}/assignee` | Update assignee |

## Evaluation Criteria

Your project will be evaluated based on the following:

| Criteria | Description |
|---|---|
| Functionality | All endpoints work as expected |
| Code Quality | Clean, readable, well-structured code |
| Authorization | Security rules enforced correctly |
| Error Handling | Proper HTTP status codes and error messages |
| Git Workflow | Meaningful commits, proper branch naming |
| Documentation | README is complete and accurate |

## Submission
- Open a Pull Request to the mentor's repository at the end of each week
- Make sure your PR description is filled out completely
- Attach your Postman collection to the final PR
