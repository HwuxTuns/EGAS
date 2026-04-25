# EGAS - Class Stubs (Khai báo classes & methods)

> ⚠️ **Lưu ý:** Tất cả methods bên dưới chỉ là **khai báo** (stub).
> Bạn sẽ implement từng method khi đến phase tương ứng.

---

## 1. Entity Classes

### User.java
```java
package com.egas.entity;

import jakarta.persistence.*;
import lombok.*;
import java.time.LocalDateTime;

@Entity
@Table(name = "users")
@Data @NoArgsConstructor @AllArgsConstructor @Builder
public class User {
    @Id @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @Column(unique = true, nullable = false)
    private String username;

    @Column(unique = true, nullable = false)
    private String email;

    @Column(nullable = false)
    private String password;

    @Enumerated(EnumType.STRING)
    private Role role; // STUDENT, TEACHER, ADMIN

    private LocalDateTime createdAt;
    private LocalDateTime updatedAt;

    public enum Role {
        STUDENT, TEACHER, ADMIN
    }

    @PrePersist
    protected void onCreate() {
        createdAt = LocalDateTime.now();
        updatedAt = LocalDateTime.now();
    }

    @PreUpdate
    protected void onUpdate() {
        updatedAt = LocalDateTime.now();
    }
}
```

### Classroom.java
```java
package com.egas.entity;

import jakarta.persistence.*;
import lombok.*;
import java.time.LocalDateTime;
import java.util.List;

@Entity
@Table(name = "classrooms")
@Data @NoArgsConstructor @AllArgsConstructor @Builder
public class Classroom {
    @Id @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String name;
    private String description;

    @ManyToOne
    @JoinColumn(name = "teacher_id")
    private User teacher;

    @OneToMany(mappedBy = "classroom")
    private List<Group> groups;

    private LocalDateTime createdAt;

    @PrePersist
    protected void onCreate() { createdAt = LocalDateTime.now(); }
}
```

### Group.java
```java
package com.egas.entity;

import jakarta.persistence.*;
import lombok.*;
import java.time.LocalDateTime;
import java.util.List;

@Entity
@Table(name = "groups_tbl") // "groups" is reserved in MySQL
@Data @NoArgsConstructor @AllArgsConstructor @Builder
public class Group {
    @Id @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String name;

    @ManyToOne
    @JoinColumn(name = "classroom_id")
    private Classroom classroom;

    @OneToMany(mappedBy = "group")
    private List<GroupMember> members;

    private LocalDateTime createdAt;

    @PrePersist
    protected void onCreate() { createdAt = LocalDateTime.now(); }
}
```

### GroupMember.java
```java
package com.egas.entity;

import jakarta.persistence.*;
import lombok.*;
import java.time.LocalDateTime;

@Entity
@Table(name = "group_members")
@Data @NoArgsConstructor @AllArgsConstructor @Builder
public class GroupMember {
    @Id @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @ManyToOne
    @JoinColumn(name = "group_id")
    private Group group;

    @ManyToOne
    @JoinColumn(name = "user_id")
    private User user;

    private LocalDateTime joinedAt;

    @PrePersist
    protected void onCreate() { joinedAt = LocalDateTime.now(); }
}
```

### Assignment.java
```java
package com.egas.entity;

import jakarta.persistence.*;
import lombok.*;
import java.time.LocalDateTime;

@Entity
@Table(name = "assignments")
@Data @NoArgsConstructor @AllArgsConstructor @Builder
public class Assignment {
    @Id @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String title;

    @Column(columnDefinition = "TEXT")
    private String description;

    @ManyToOne
    @JoinColumn(name = "classroom_id")
    private Classroom classroom;

    private LocalDateTime deadline;
    private String githubRepoUrl;
    private LocalDateTime createdAt;

    @PrePersist
    protected void onCreate() { createdAt = LocalDateTime.now(); }
}
```

### Submission.java
```java
package com.egas.entity;

import jakarta.persistence.*;
import lombok.*;
import java.time.LocalDateTime;

@Entity
@Table(name = "submissions")
@Data @NoArgsConstructor @AllArgsConstructor @Builder
public class Submission {
    @Id @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @ManyToOne
    @JoinColumn(name = "assignment_id")
    private Assignment assignment;

    @ManyToOne
    @JoinColumn(name = "group_id")
    private Group group;

    private String githubUrl;
    private LocalDateTime submittedAt;

    @Enumerated(EnumType.STRING)
    private Status status; // PENDING, REVIEWED, GRADED

    public enum Status {
        PENDING, REVIEWED, GRADED
    }

    @PrePersist
    protected void onCreate() { submittedAt = LocalDateTime.now(); }
}
```

### Grade.java
```java
package com.egas.entity;

import jakarta.persistence.*;
import lombok.*;
import java.time.LocalDateTime;

@Entity
@Table(name = "grades")
@Data @NoArgsConstructor @AllArgsConstructor @Builder
public class Grade {
    @Id @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @ManyToOne
    @JoinColumn(name = "submission_id")
    private Submission submission;

    private Double score;

    @Enumerated(EnumType.STRING)
    private GradingType gradingType; // AI, TEACHER, COMMIT_BASED

    @Column(columnDefinition = "TEXT")
    private String feedback;

    private LocalDateTime gradedAt;

    public enum GradingType {
        AI, TEACHER, COMMIT_BASED
    }

    @PrePersist
    protected void onCreate() { gradedAt = LocalDateTime.now(); }
}
```

### AiReview.java
```java
package com.egas.entity;

import jakarta.persistence.*;
import lombok.*;
import java.time.LocalDateTime;

@Entity
@Table(name = "ai_reviews")
@Data @NoArgsConstructor @AllArgsConstructor @Builder
public class AiReview {
    @Id @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @ManyToOne
    @JoinColumn(name = "submission_id")
    private Submission submission;

    @Column(columnDefinition = "TEXT")
    private String reviewContent;

    private LocalDateTime createdAt;

    @PrePersist
    protected void onCreate() { createdAt = LocalDateTime.now(); }
}
```

---

## 2. Repository Interfaces

```java
// UserRepository.java
package com.egas.repository;
import com.egas.entity.User;
import org.springframework.data.jpa.repository.JpaRepository;
import java.util.Optional;

public interface UserRepository extends JpaRepository<User, Long> {
    Optional<User> findByUsername(String username);
    Optional<User> findByEmail(String email);
    boolean existsByUsername(String username);
    boolean existsByEmail(String email);
}

// ClassroomRepository.java
public interface ClassroomRepository extends JpaRepository<Classroom, Long> {
    List<Classroom> findByTeacherId(Long teacherId);
}

// GroupRepository.java
public interface GroupRepository extends JpaRepository<Group, Long> {
    List<Group> findByClassroomId(Long classroomId);
}

// GroupMemberRepository.java
public interface GroupMemberRepository extends JpaRepository<GroupMember, Long> {
    List<GroupMember> findByGroupId(Long groupId);
    List<GroupMember> findByUserId(Long userId);
    boolean existsByGroupIdAndUserId(Long groupId, Long userId);
}

// AssignmentRepository.java
public interface AssignmentRepository extends JpaRepository<Assignment, Long> {
    List<Assignment> findByClassroomId(Long classroomId);
}

// SubmissionRepository.java
public interface SubmissionRepository extends JpaRepository<Submission, Long> {
    List<Submission> findByAssignmentId(Long assignmentId);
    List<Submission> findByGroupId(Long groupId);
    Optional<Submission> findByAssignmentIdAndGroupId(Long assignmentId, Long groupId);
}

// GradeRepository.java
public interface GradeRepository extends JpaRepository<Grade, Long> {
    List<Grade> findBySubmissionId(Long submissionId);
}

// AiReviewRepository.java
public interface AiReviewRepository extends JpaRepository<AiReview, Long> {
    Optional<AiReview> findBySubmissionId(Long submissionId);
}
```

---

## 3. DTO Classes

```java
// === Request DTOs ===

// dto/request/RegisterRequest.java
public record RegisterRequest(
    String username,
    String email,
    String password,
    String role     // "STUDENT" or "TEACHER"
) {}

// dto/request/LoginRequest.java
public record LoginRequest(
    String username,
    String password
) {}

// dto/request/ClassroomRequest.java
public record ClassroomRequest(
    String name,
    String description
) {}

// dto/request/AssignmentRequest.java
public record AssignmentRequest(
    String title,
    String description,
    Long classroomId,
    LocalDateTime deadline,
    String githubRepoUrl
) {}

// dto/request/SubmissionRequest.java
public record SubmissionRequest(
    Long assignmentId,
    Long groupId,
    String githubUrl
) {}

// === Response DTOs ===

// dto/response/ApiResponse.java
public record ApiResponse<T>(
    boolean success,
    T data,
    String message
) {
    public static <T> ApiResponse<T> ok(T data, String message) {
        return new ApiResponse<>(true, data, message);
    }
    public static <T> ApiResponse<T> error(String message) {
        return new ApiResponse<>(false, null, message);
    }
}

// dto/response/AuthResponse.java
public record AuthResponse(
    String token,
    String username,
    String role
) {}

// dto/response/UserResponse.java
public record UserResponse(
    Long id,
    String username,
    String email,
    String role
) {}
```

---

## 4. Service Interfaces & Implementations

```java
// === service/interfaces/AuthService.java ===
public interface AuthService {
    AuthResponse register(RegisterRequest request);
    AuthResponse login(LoginRequest request);
}

// === service/impl/AuthServiceImpl.java ===
@Service
@RequiredArgsConstructor
public class AuthServiceImpl implements AuthService {
    private final UserRepository userRepository;
    // TODO: inject PasswordEncoder, JwtUtil

    @Override
    public AuthResponse register(RegisterRequest request) {
        // TODO: Phase 1 - Implement registration logic
        throw new UnsupportedOperationException("Not implemented yet");
    }

    @Override
    public AuthResponse login(LoginRequest request) {
        // TODO: Phase 1 - Implement login logic
        throw new UnsupportedOperationException("Not implemented yet");
    }
}

// === service/interfaces/ClassroomService.java ===
public interface ClassroomService {
    Classroom create(ClassroomRequest request, Long teacherId);
    List<Classroom> getByTeacher(Long teacherId);
    Classroom getById(Long id);
    Classroom update(Long id, ClassroomRequest request);
    void delete(Long id);
}

// === service/interfaces/AssignmentService.java ===
public interface AssignmentService {
    Assignment create(AssignmentRequest request);
    List<Assignment> getByClassroom(Long classroomId);
    Assignment getById(Long id);
    Assignment update(Long id, AssignmentRequest request);
    void delete(Long id);
}

// === service/interfaces/SubmissionService.java ===
public interface SubmissionService {
    Submission submit(SubmissionRequest request);
    List<Submission> getByAssignment(Long assignmentId);
    Submission getById(Long id);
}

// === service/interfaces/GithubService.java ===
public interface GithubService {
    String createRepository(String repoName);
    void pushCode(String repoUrl, String code);
    List<String> getCommitHistory(String repoUrl);
}

// === service/interfaces/GradingService.java ===
public interface GradingService {
    Grade gradeSubmission(Long submissionId, String strategyType);
}
```

---

## 5. Strategy Pattern (Chấm điểm)

```java
// === service/strategy/GradingStrategy.java ===
public interface GradingStrategy {
    Grade grade(Submission submission);
}

// === service/strategy/AIGradingStrategy.java ===
@Component
public class AIGradingStrategy implements GradingStrategy {
    @Override
    public Grade grade(Submission submission) {
        // TODO: Phase 7 - Call Gemini API to review code
        throw new UnsupportedOperationException("Not implemented yet");
    }
}

// === service/strategy/TeacherGradingStrategy.java ===
@Component
public class TeacherGradingStrategy implements GradingStrategy {
    @Override
    public Grade grade(Submission submission) {
        // TODO: Phase 7 - Manual grading by teacher
        throw new UnsupportedOperationException("Not implemented yet");
    }
}

// === service/strategy/CommitBasedGradingStrategy.java ===
@Component
public class CommitBasedGradingStrategy implements GradingStrategy {
    @Override
    public Grade grade(Submission submission) {
        // TODO: Phase 7 - Grade based on commit frequency/quality
        throw new UnsupportedOperationException("Not implemented yet");
    }
}
```

---

## 6. Factory Pattern (Git Service)

```java
// === service/factory/GitServiceFactory.java ===
@Component
public class GitServiceFactory {
    public GithubService create(String type) {
        // TODO: Phase 6 - Return appropriate Git service
        // "github" -> GithubServiceImpl
        // "gitlab" -> GitlabServiceImpl (tương lai)
        throw new UnsupportedOperationException("Not implemented yet");
    }
}
```

---

## 7. Controllers

```java
// === controller/AuthController.java ===
@RestController
@RequestMapping("/api/auth")
@RequiredArgsConstructor
public class AuthController {
    private final AuthService authService;

    @PostMapping("/register")
    public ResponseEntity<ApiResponse<AuthResponse>> register(
            @RequestBody RegisterRequest request) {
        // TODO: Phase 1
        return null;
    }

    @PostMapping("/login")
    public ResponseEntity<ApiResponse<AuthResponse>> login(
            @RequestBody LoginRequest request) {
        // TODO: Phase 1
        return null;
    }
}

// === controller/ClassroomController.java ===
@RestController
@RequestMapping("/api/classrooms")
@RequiredArgsConstructor
public class ClassroomController {
    private final ClassroomService classroomService;

    @PostMapping
    public ResponseEntity<?> create(@RequestBody ClassroomRequest request) {
        // TODO: Phase 2
        return null;
    }

    @GetMapping
    public ResponseEntity<?> getAll() {
        // TODO: Phase 2
        return null;
    }

    @GetMapping("/{id}")
    public ResponseEntity<?> getById(@PathVariable Long id) {
        // TODO: Phase 2
        return null;
    }

    @PutMapping("/{id}")
    public ResponseEntity<?> update(@PathVariable Long id,
            @RequestBody ClassroomRequest request) {
        // TODO: Phase 2
        return null;
    }

    @DeleteMapping("/{id}")
    public ResponseEntity<?> delete(@PathVariable Long id) {
        // TODO: Phase 2
        return null;
    }
}

// === controller/AssignmentController.java ===
@RestController
@RequestMapping("/api/assignments")
@RequiredArgsConstructor
public class AssignmentController {
    private final AssignmentService assignmentService;

    @PostMapping
    public ResponseEntity<?> create(@RequestBody AssignmentRequest request) {
        // TODO: Phase 4
        return null;
    }

    @GetMapping("/classroom/{classroomId}")
    public ResponseEntity<?> getByClassroom(@PathVariable Long classroomId) {
        // TODO: Phase 4
        return null;
    }

    @GetMapping("/{id}")
    public ResponseEntity<?> getById(@PathVariable Long id) {
        // TODO: Phase 4
        return null;
    }

    @PutMapping("/{id}")
    public ResponseEntity<?> update(@PathVariable Long id,
            @RequestBody AssignmentRequest request) {
        // TODO: Phase 4
        return null;
    }

    @DeleteMapping("/{id}")
    public ResponseEntity<?> delete(@PathVariable Long id) {
        // TODO: Phase 4
        return null;
    }
}

// === controller/SubmissionController.java ===
@RestController
@RequestMapping("/api/submissions")
@RequiredArgsConstructor
public class SubmissionController {
    private final SubmissionService submissionService;

    @PostMapping
    public ResponseEntity<?> submit(@RequestBody SubmissionRequest request) {
        // TODO: Phase 5
        return null;
    }

    @GetMapping("/assignment/{assignmentId}")
    public ResponseEntity<?> getByAssignment(@PathVariable Long assignmentId) {
        // TODO: Phase 5
        return null;
    }

    @GetMapping("/{id}")
    public ResponseEntity<?> getById(@PathVariable Long id) {
        // TODO: Phase 5
        return null;
    }
}
```

---

## 8. Exception Handling

```java
// === exception/ResourceNotFoundException.java ===
public class ResourceNotFoundException extends RuntimeException {
    public ResourceNotFoundException(String message) {
        super(message);
    }
}

// === exception/DuplicateResourceException.java ===
public class DuplicateResourceException extends RuntimeException {
    public DuplicateResourceException(String message) {
        super(message);
    }
}

// === exception/GlobalExceptionHandler.java ===
@RestControllerAdvice
public class GlobalExceptionHandler {
    @ExceptionHandler(ResourceNotFoundException.class)
    public ResponseEntity<ApiResponse<?>> handleNotFound(ResourceNotFoundException ex) {
        // TODO: Phase 9 - Return proper error response
        return null;
    }

    @ExceptionHandler(Exception.class)
    public ResponseEntity<ApiResponse<?>> handleGeneral(Exception ex) {
        // TODO: Phase 9 - Return generic error response
        return null;
    }
}
```

---

## 9. Security & Config

```java
// === config/SecurityConfig.java ===
@Configuration
@EnableWebSecurity
public class SecurityConfig {
    // TODO: Phase 1 - Configure SecurityFilterChain
    // - Permit /api/auth/** without authentication
    // - Require auth for everything else
    // - Add JWT filter
}

// === security/JwtUtil.java ===
@Component
public class JwtUtil {
    public String generateToken(String username) {
        // TODO: Phase 1
        throw new UnsupportedOperationException("Not implemented yet");
    }

    public String extractUsername(String token) {
        // TODO: Phase 1
        throw new UnsupportedOperationException("Not implemented yet");
    }

    public boolean validateToken(String token) {
        // TODO: Phase 1
        throw new UnsupportedOperationException("Not implemented yet");
    }
}

// === security/JwtAuthFilter.java ===
@Component
public class JwtAuthFilter extends OncePerRequestFilter {
    @Override
    protected void doFilterInternal(HttpServletRequest request,
            HttpServletResponse response, FilterChain filterChain) {
        // TODO: Phase 1 - Extract and validate JWT from header
    }
}
```
