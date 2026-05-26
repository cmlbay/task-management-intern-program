# Week 5 — Testing (Optional)

## Goal
Write unit and integration tests for the application.
By the end of this week, your codebase will have meaningful
test coverage and you will understand how to test Spring Boot applications.

## Topics
- JUnit 5 basics
- Mockito for mocking dependencies
- Integration testing with @SpringBootTest
- Test coverage measurement

## Tasks

### 1. Add Testing Dependencies

Add the following to your `pom.xml` if not already present:

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-test</artifactId>
    <scope>test</scope>
</dependency>
```

### 2. Write Unit Tests for TaskService

Create `TaskServiceTest.java` under `src/test/java/.../service/`:

```java
@ExtendWith(MockitoExtension.class)
class TaskServiceTest {

    @Mock
    private TaskRepository taskRepository;

    @Mock
    private ProjectRepository projectRepository;

    @InjectMocks
    private TaskService taskService;

    @Test
    void shouldCreateTaskSuccessfully() {
        // given
        // when
        // then
    }

    @Test
    void shouldThrowForbiddenWhenUserIsNotProjectMember() {
        // given
        // when
        // then
    }

    @Test
    void shouldThrowExceptionWhenTaskStatusSkipsStep() {
        // given
        // when
        // then
    }
}
```

### 3. Write Unit Tests for ProjectService

Create `ProjectServiceTest.java` under `src/test/java/.../service/`:

- Test successful project creation
- Test that only owner can update project
- Test that only owner can archive project
- Test that only owner or manager can add members

### 4. Write Integration Tests

Create `TaskControllerIntegrationTest.java`:

```java
@SpringBootTest
@AutoConfigureMockMvc
class TaskControllerIntegrationTest {

    @Autowired
    private MockMvc mockMvc;

    @Test
    void shouldReturn401WhenNotAuthenticated() throws Exception {
        mockMvc.perform(get("/api/v1/projects/uuid/tasks"))
            .andExpect(status().isUnauthorized());
    }

    @Test
    void shouldReturn403WhenUserIsNotProjectMember() throws Exception {
        // setup
        // perform
        // assert
    }
}
```

### 5. Measure Test Coverage

Run the following command to generate a coverage report:

```bash
mvn test jacoco:report
```

Open `target/site/jacoco/index.html` in your browser to see the coverage report.

**Target:** Minimum 60% coverage on service layer.

## Deliverables
- [ ] Unit tests written for TaskService
- [ ] Unit tests written for ProjectService
- [ ] Mockito used to mock dependencies
- [ ] Integration tests written with @SpringBootTest
- [ ] Test coverage is above 60%
- [ ] All tests pass with `mvn test`
- [ ] PR opened with branch name `feature/week-5-testing`

## Resources
- [JUnit 5 Documentation](https://junit.org/junit5/docs/current/user-guide/)
- [Mockito Documentation](https://javadoc.io/doc/org.mockito/mockito-core/latest/org/mockito/Mockito.html)
- [Baeldung — Spring Boot Testing](https://www.baeldung.com/spring-boot-testing)
- [JaCoCo Coverage](https://www.eclemma.org/jacoco/)
