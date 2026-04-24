Hệ thống Website Quản lý Bài tập Nhóm Tin học (THPT)
1. Giới thiệu dự án

Đây là dự án xây dựng hệ thống Website quản lý bài tập nhóm môn Tin học dành cho học sinh THPT. Hệ thống hỗ trợ giáo viên giao bài tập, học sinh làm việc theo nhóm và nộp bài trực tuyến thông qua tích hợp với GitHub.

Ngoài ra, hệ thống còn tích hợp AI (Gemini) để tự động review code, hỗ trợ đánh giá chất lượng bài làm ngay sau khi học sinh nộp bài.

Dự án được xây dựng theo kiến trúc Backend tập trung với:

Backend: xây dựng bằng Spring Boot (Java), sử dụng MySQL làm cơ sở dữ liệu.
Hệ thống tích hợp API bên ngoài: GitHub API và Gemini AI.

Kiến trúc backend được thiết kế theo mô hình Layered Architecture kết hợp với các nguyên tắc SOLID và các Design Pattern nhằm đảm bảo hệ thống dễ bảo trì, dễ mở rộng và rõ ràng trong việc phân tách trách nhiệm.

2. Mục tiêu của dự án

Mục tiêu của dự án:

Xây dựng hệ thống quản lý bài tập nhóm cho học sinh THPT.
Tích hợp Git để quản lý source code bài nộp.
Tích hợp AI để hỗ trợ chấm điểm và review code.
Áp dụng kiến trúc nhiều tầng (Layered Architecture).
Áp dụng các nguyên tắc thiết kế phần mềm SOLID.
Áp dụng Design Pattern (Strategy, Factory, Repository).
Tổ chức code theo hướng dễ bảo trì và dễ mở rộng.
3. Chức năng chính của hệ thống
3.1 Chức năng dành cho học sinh
Đăng ký tài khoản
Đăng nhập / đăng xuất
Xem danh sách bài tập
Tham gia nhóm
Xem chi tiết bài tập
Nộp bài (upload source code)
Xem kết quả chấm điểm
Xem nhận xét từ AI
3.2 Chức năng dành cho giáo viên
Tạo bài tập
Quản lý lớp học
Quản lý nhóm học sinh
Theo dõi tiến độ làm bài
Chấm điểm thủ công
Xem kết quả chấm từ AI
Xem lịch sử nộp bài
3.3 Quản lý bài tập (Assignment)

Thông tin mỗi bài tập bao gồm:

Tiêu đề bài tập
Mô tả
Hạn nộp
Repository GitHub
Tiêu chí chấm điểm
Danh sách nhóm tham gia
3.4 Quản lý nộp bài (Submission)
Nộp bài thông qua hệ thống
Push code lên GitHub
Lưu lịch sử nộp bài
Tự động trigger AI review
Lưu kết quả chấm điểm
3.5 Chức năng bổ sung
Tích hợp AI (Gemini) để review code
Hệ thống chấm điểm đa chiến lược (Strategy Pattern)
Theo dõi commit từ GitHub
3.6 Trang quản trị
Quản lý người dùng
Quản lý lớp học
Quản lý bài tập
Quản lý bài nộp
Theo dõi hệ thống
4. Kiến trúc hệ thống

Backend được xây dựng theo mô hình Layered Architecture gồm các tầng:

Controller: Nhận request từ client và trả response.
Service: Xử lý logic nghiệp vụ.
Repository: Tương tác với cơ sở dữ liệu (MySQL).
DTO: Truyền dữ liệu giữa các tầng.
Entity: Định nghĩa cấu trúc bảng database.

Luồng xử lý cơ bản:

Client → Controller → Service → Repository → Database

Ngoài ra, hệ thống áp dụng các Design Pattern:

Strategy Pattern: Cho chấm điểm (AI, giáo viên, commit)
Factory Pattern: Tạo Git Service (GitHub, GitLab trong tương lai)
Repository Pattern: Truy xuất dữ liệu

Kiến trúc này giúp:

Phân tách trách nhiệm rõ ràng
Dễ kiểm thử
Dễ mở rộng hệ thống
5. Cấu trúc thư mục dự án (kèm chú thích chức năng)
edu-group-assignment-system
│
├── src/main/java/com/project
│
│   ├── controller
│   │   AssignmentController.java
│   │   SubmissionController.java
│   │   UserController.java
│   │
│   │   # Chức năng:
│   │   - Nhận request từ client
│   │   - Gọi Service xử lý
│   │   - Trả response
│
│   ├── service
│   │
│   │   ├── interfaces
│   │   │   AssignmentService.java
│   │   │   SubmissionService.java
│   │   │   GithubService.java
│   │
│   │   ├── impl
│   │   │   AssignmentServiceImpl.java
│   │   │   SubmissionServiceImpl.java
│   │   │   GithubServiceImpl.java
│   │
│   │   ├── strategy
│   │   │   GradingStrategy.java
│   │   │   AIGradingStrategy.java
│   │   │   TeacherGradingStrategy.java
│   │   │   CommitBasedGradingStrategy.java
│   │
│   │   ├── factory
│   │   │   GitServiceFactory.java
│   │
│   │   # Chức năng:
│   │   - Xử lý business logic
│   │   - Điều phối chấm điểm
│   │   - Tích hợp GitHub & AI
│
│   ├── repository
│   │   UserRepository.java
│   │   AssignmentRepository.java
│   │   SubmissionRepository.java
│   │
│   │   # Chức năng:
│   │   - CRUD database
│   │   - Không chứa business logic
│
│   ├── entity
│   │   User.java
│   │   Classroom.java
│   │   Group.java
│   │   Assignment.java
│   │   Submission.java
│   │
│   │   # Chức năng:
│   │   - Mapping bảng MySQL
│
│   ├── dto
│   │   AssignmentDTO.java
│   │   SubmissionDTO.java
│   │
│   │   # Chức năng:
│   │   - Truyền dữ liệu giữa các tầng
│
│   ├── config
│   │   SecurityConfig.java
│   │   GithubConfig.java
│   │
│   │   # Chức năng:
│   │   - Cấu hình hệ thống
│
│   └── Application.java
│
│       # Chức năng:
│       - Khởi chạy Spring Boot
│
├── docker-compose.yml
│
│   # Chức năng:
│   # Chạy MySQL bằng Docker
│
├── src/main/resources
│   application.yml
│
└── README.md
6. Hướng dẫn chạy dự án
Bước 1: Clone repository
git clone <repository-url>
cd edu-group-assignment-system
Bước 2: Chạy MySQL bằng Docker
docker-compose up -d

(Sử dụng Docker)

Bước 3: Cấu hình application.yml

Điền thông tin:

MySQL
GitHub Token
Gemini API Key
Bước 4: Chạy backend
./mvnw spring-boot:run
7. Chuẩn API Response
{
  "success": true,
  "data": {},
  "message": "Success"
}
8. Quy tắc làm việc nhóm
Không push trực tiếp vào main.
Mỗi thành viên làm việc trên một branch riêng.
Tạo Pull Request trước khi merge.

Ví dụ:

feat/assignment
feat/submission
feat/github-integration
feat/ai-grading
9. Công nghệ sử dụng

Backend:

Java Spring Boot
Spring Data JPA
MySQL

Tích hợp:

GitHub API
Gemini AI (Google)

Công cụ:

Docker
Git
GitHub
10. Mục đích dự án

Dự án được thực hiện nhằm mục đích:

Học tập và áp dụng kiến trúc phần mềm hiện đại
Thực hành tích hợp API bên ngoài (Git, AI)
Áp dụng Design Pattern trong thực tế
Xây dựng hệ thống có khả năng mở rộng và bảo trì cao
