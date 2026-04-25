# Hệ thống Website Quản lý Bài tập Nhóm Tin học (THPT)

---

## 1. Giới thiệu dự án

Đây là dự án xây dựng hệ thống Website quản lý bài tập nhóm môn Tin học dành cho học sinh THPT. 
Hệ thống hỗ trợ giáo viên giao bài tập, học sinh làm việc theo nhóm và nộp bài trực tuyến thông qua tích hợp với GitHub.

Ngoài ra, hệ thống tích hợp AI nhằm hỗ trợ quá trình học tập và đánh giá, đóng vai trò như một **trợ lý thông minh** cho cả giáo viên và học sinh.

Dự án được xây dựng bằng:

* Backend: Java Spring Boot
* Database: MySQL (chạy bằng Docker)
* Tích hợp: GitHub API và AI APIs (có khả năng mở rộng)

---

## 2. Mục tiêu của dự án

* Xây dựng hệ thống quản lý bài tập nhóm cho học sinh THPT
* Tích hợp Git để quản lý source code bài nộp
* Tích hợp AI hỗ trợ học tập và đánh giá
* Áp dụng Layered Architecture và SOLID
* Áp dụng Design Pattern (Strategy, Factory, Repository)
* Thiết kế hệ thống dễ mở rộng (AI, Git providers)

---

## 3. Chức năng chính của hệ thống

### 3.1 Chức năng dành cho học sinh

* Đăng ký tài khoản
* Đăng nhập / đăng xuất
* Xem danh sách bài tập
* Tham gia nhóm
* Xem chi tiết bài tập
* Nộp bài (upload source code)
* Xem điểm và nhận xét
* Chat với AI để được gợi ý làm bài

---

### 3.2 Chức năng dành cho giáo viên

* Tạo bài tập
* Quản lý lớp học
* Quản lý nhóm học sinh
* Theo dõi tiến độ làm bài
* Chấm điểm thủ công
* Xem gợi ý chấm điểm từ AI
* Xem lịch sử nộp bài

---

### 3.3 Quản lý bài tập (Assignment)

Thông tin mỗi bài tập bao gồm:

* Tiêu đề bài tập
* Mô tả
* Hạn nộp
* Repository GitHub
* Tiêu chí chấm điểm
* Danh sách nhóm tham gia

---

### 3.4 Quản lý bài nộp (Submission)

* Nộp bài thông qua hệ thống
* Push code lên GitHub
* Lưu lịch sử nộp bài
* Tự động trigger AI phân tích
* Lưu kết quả gợi ý

---

### 3.5 Tích hợp AI (Điểm nổi bật)

Hệ thống được thiết kế theo hướng **mở rộng AI**, không phụ thuộc vào một nền tảng duy nhất.

#### Có thể tích hợp:

* Gemini (Google)
* OpenAI (GPT)
* Các AI khác trong tương lai

#### Kiến trúc:

* Sử dụng **Strategy Pattern** để lựa chọn AI
* Sử dụng **Factory Pattern** để khởi tạo AI Service
* Dễ dàng thay thế hoặc thêm AI mới mà không ảnh hưởng hệ thống

---

### 🎯 Vai trò của AI

#### Đối với giáo viên:

* Gợi ý điểm số (AI grading suggestion)
* Gợi ý nhận xét bài làm
* Phân tích chất lượng code (clean code, logic, cấu trúc)

#### Đối với học sinh:

* Chat trực tiếp với AI
* AI có khả năng đọc source code đã nộp
* Gợi ý hướng giải bài
* Phát hiện lỗi và giải thích lỗi
* Đưa ra cách cải thiện code

👉 Lưu ý:  
AI chỉ đóng vai trò **gợi ý (assistant)**, không thay thế quyết định của giáo viên.

---

### 3.6 Trang quản trị

* Quản lý người dùng
* Quản lý lớp học
* Quản lý bài tập
* Quản lý bài nộp
* Theo dõi hệ thống

---

## 4. Kiến trúc hệ thống

Backend được xây dựng theo mô hình **Layered Architecture**:
Client → Controller → Service → Repository → Database


---

### Áp dụng Design Pattern

* **Strategy Pattern**:
  - Chấm điểm (AI, giáo viên, commit)

* **Factory Pattern**:
  - Tạo AI Service
  - Tạo Git Service (GitHub, GitLab trong tương lai)

* **Repository Pattern**:
  - Truy xuất dữ liệu

---

## 5. Cấu trúc thư mục dự án

edu_group_assignment_system
│
├── controller
│   ├── assignment
│   │   AssignmentController.java
│   │
│   ├── submission
│   │   SubmissionController.java
│   │
│   └── user
│       UserController.java
│
├── service
│   ├── assignment
│   │   AssignmentService.java
│   │   AssignmentServiceImpl.java
│   │
│   ├── submission
│   │   SubmissionService.java
│   │   SubmissionServiceImpl.java
│   │
│   ├── ai
│   │   AIService.java
│   │   impl/
│   │       GeminiService.java
│   │       OpenAIService.java
│   │
│   ├── github
│   │   GithubService.java
│   │   GithubServiceImpl.java
│
│   ├── strategy
│   │   GradingStrategy.java
│   │   impl/
│   │       AIGradingStrategy.java
│   │       TeacherGradingStrategy.java
│   │       CommitGradingStrategy.java
│
│   ├── factory
│   │   GradingStrategyFactory.java
│   │   AIServiceFactory.java
│
├── repository
│   AssignmentRepository.java
│   SubmissionRepository.java
│   UserRepository.java
│
├── entity
│   User.java
│   Classroom.java
│   Group.java
│   Assignment.java
│   Submission.java
│
├── dto
│   assignment/
│   submission/
│   user/
│
├── config
│   SecurityConfig.java
│   GithubConfig.java
│
└── EduApplication.java


---

## 6. Hướng dẫn chạy dự án

### Bước 1: Clone repository
git clone <repository-url>
cd project


---

### Bước 2: Chạy MySQL bằng Docker
docker-compose up -d


---

### Bước 3: Cấu hình application.yml

Cấu hình các thông tin:

* Database (MySQL)
* GitHub Personal Access Token
* AI API Key (Gemini / OpenAI)

---

### Bước 4: Chạy backend
./mvnw spring-boot:run


---

## 7. Chuẩn API Response
{
"success": true,
"data": {},
"message": "Success"
}


---

## 8. Quy tắc làm việc nhóm

* Không push trực tiếp vào main
* Mỗi thành viên làm việc trên một branch riêng
* Tạo Pull Request trước khi merge

Ví dụ:
feat/assignment
feat/submission
feat/github-integration
feat/ai-service


---

## 9. Công nghệ sử dụng

Backend:

* Java Spring Boot
* Spring Data JPA
* MySQL

Tích hợp:

* GitHub API
* AI APIs (Gemini, OpenAI, ...)

Công cụ:

* Docker
* Git
* GitHub

---

## 10. Mục đích dự án

* Học tập và áp dụng kiến trúc phần mềm hiện đại
* Thực hành tích hợp API (Git, AI)
* Áp dụng Design Pattern trong thực tế
* Xây dựng hệ thống có khả năng mở rộng và bảo trì cao
