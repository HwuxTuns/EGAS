# EGAS - Tổng quan Kiến trúc Dự án

## 1. Kiến trúc tổng thể

```
┌─────────────┐     ┌──────────────────┐     ┌─────────┐
│  Frontend   │────▶│    Backend API   │────▶│  MySQL  │
│  (Thymeleaf │     │  (Spring Boot)   │     │   DB    │
│   or React) │     │   Port: 8080     │     │  :3306  │
└─────────────┘     └──────┬───────────┘     └─────────┘
                           │
                    ┌──────┴───────┐
                    │  External    │
                    │  - GitHub API│
                    │  - Gemini AI │
                    └──────────────┘
```

## 2. Tổ chức Backend (Spring Boot)

```
src/main/java/com/egas/
├── EgasApplication.java          # Main class
├── config/                       # Cấu hình (Security, GitHub, AI...)
├── controller/                   # REST API endpoints
├── service/
│   ├── interfaces/               # Service interfaces
│   ├── impl/                     # Service implementations
│   ├── strategy/                 # Strategy pattern (chấm điểm)
│   └── factory/                  # Factory pattern (Git service)
├── repository/                   # Spring Data JPA repositories
├── entity/                       # JPA entities (mapping DB)
├── dto/                          # Data Transfer Objects
│   ├── request/                  # Request DTOs
│   └── response/                 # Response DTOs
├── exception/                    # Custom exceptions + handler
├── security/                     # JWT, filters
└── util/                         # Helper/utility classes
```

## 3. Tổ chức Frontend

> Gợi ý: Bắt đầu với **Thymeleaf** (server-side rendering) để đơn giản.
> Sau khi hoàn thiện backend, có thể chuyển sang React/Vue nếu muốn.

```
src/main/resources/
├── templates/                    # Thymeleaf HTML templates
│   ├── layout/                   # Base layouts
│   ├── auth/                     # Login, Register
│   ├── dashboard/                # Trang chính
│   ├── assignment/               # CRUD bài tập
│   ├── submission/               # Nộp bài, xem kết quả
│   └── admin/                    # Trang quản trị
├── static/
│   ├── css/
│   ├── js/
│   └── images/
└── application.yml
```

## 4. Thiết kế Database (MySQL)

```sql
-- Bảng người dùng
users (id, username, email, password, role, created_at, updated_at)

-- Bảng lớp học
classrooms (id, name, description, teacher_id, created_at)

-- Bảng nhóm
groups (id, name, classroom_id, created_at)

-- Bảng thành viên nhóm
group_members (id, group_id, user_id, joined_at)

-- Bảng bài tập
assignments (id, title, description, classroom_id, deadline, github_repo_url, created_at)

-- Bảng bài nộp
submissions (id, assignment_id, group_id, github_url, submitted_at, status)

-- Bảng kết quả chấm điểm
grades (id, submission_id, score, grading_type, feedback, graded_at)

-- Bảng AI review
ai_reviews (id, submission_id, review_content, created_at)
```
