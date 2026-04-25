# EGAS - Phân chia Phase & Task

## Phase 0: Khởi tạo dự án (Setup) ⏱️ ~1 ngày
- [x] Tạo README.md
- [ ] Khởi tạo Spring Boot project (Maven)
- [ ] Cấu hình `application.yml` (MySQL, JPA)
- [ ] Tạo `docker-compose.yml` cho MySQL
- [ ] Tạo cấu trúc thư mục (packages)
- [ ] Tạo các entity classes (chỉ khai báo fields)
- [ ] Chạy thử → DB tự tạo bảng (ddl-auto: update)

## Phase 1: Authentication (Đăng ký / Đăng nhập) ⏱️ ~3-4 ngày
- [ ] Tạo `User` entity + repository
- [ ] Tạo `AuthController` (register, login endpoints)
- [ ] Tạo `AuthService` interface + impl
- [ ] Tạo Request/Response DTOs (RegisterRequest, LoginRequest)
- [ ] Cấu hình Spring Security cơ bản
- [ ] Hash password với BCrypt
- [ ] Implement JWT token (tạo + validate)
- [ ] Tạo trang Login/Register (Thymeleaf)
- [ ] Test đăng ký + đăng nhập

## Phase 2: Quản lý Lớp học ⏱️ ~2-3 ngày
- [ ] Tạo `Classroom` entity + repository
- [ ] Tạo `ClassroomController` (CRUD endpoints)
- [ ] Tạo `ClassroomService` interface + impl
- [ ] Tạo DTOs cho Classroom
- [ ] Giao viên: tạo/sửa/xóa lớp
- [ ] Học sinh: xem danh sách lớp, tham gia lớp
- [ ] Tạo giao diện quản lý lớp học

## Phase 3: Quản lý Nhóm ⏱️ ~2-3 ngày
- [ ] Tạo `Group` + `GroupMember` entity + repository
- [ ] Tạo `GroupController` + `GroupService`
- [ ] Tạo/tham gia/rời nhóm
- [ ] Giới hạn số thành viên nhóm
- [ ] Giao diện quản lý nhóm

## Phase 4: Quản lý Bài tập (Assignment) ⏱️ ~3-4 ngày
- [ ] Tạo `Assignment` entity + repository
- [ ] Tạo `AssignmentController` + `AssignmentService`
- [ ] CRUD bài tập (giáo viên)
- [ ] Xem danh sách + chi tiết bài tập (học sinh)
- [ ] Gắn bài tập với lớp học
- [ ] Thiết lập deadline
- [ ] Giao diện bài tập

## Phase 5: Nộp bài (Submission) ⏱️ ~3-4 ngày
- [ ] Tạo `Submission` entity + repository
- [ ] Tạo `SubmissionController` + `SubmissionService`
- [ ] Upload/nộp bài
- [ ] Lưu lịch sử nộp bài
- [ ] Xem trạng thái bài nộp
- [ ] Giao diện nộp bài + lịch sử

## Phase 6: Tích hợp GitHub ⏱️ ~4-5 ngày
- [ ] Cấu hình GitHub API (token, config)
- [ ] Tạo `GithubService` interface + impl
- [ ] Tạo `GitServiceFactory` (Factory Pattern)
- [ ] Tạo repository trên GitHub cho bài tập
- [ ] Push code bài nộp lên GitHub
- [ ] Theo dõi commit history
- [ ] Hiển thị thông tin GitHub trên giao diện

## Phase 7: Tích hợp AI & Chấm điểm ⏱️ ~5-6 ngày
- [ ] Tạo `GradingStrategy` interface (Strategy Pattern)
- [ ] Implement `AIGradingStrategy` (Gemini API)
- [ ] Implement `TeacherGradingStrategy`
- [ ] Implement `CommitBasedGradingStrategy`
- [ ] Tạo `Grade` + `AiReview` entity + repository
- [ ] Auto trigger AI review khi nộp bài
- [ ] Hiển thị kết quả chấm + nhận xét AI

## Phase 8: Trang quản trị (Admin) ⏱️ ~3-4 ngày
- [ ] Dashboard thống kê
- [ ] Quản lý users
- [ ] Quản lý lớp/nhóm/bài tập
- [ ] Xem logs hệ thống

## Phase 9: Hoàn thiện & Tối ưu ⏱️ ~3-4 ngày
- [ ] Validation toàn bộ input
- [ ] Global exception handling
- [ ] Logging (SLF4J)
- [ ] Unit tests cho Service layer
- [ ] Tối ưu queries (N+1 problem)
- [ ] API documentation (Swagger)
- [ ] README cập nhật cuối cùng

---

> 💡 **Tổng ước lượng: ~25-35 ngày làm việc**
> Mỗi phase hoàn thành = 1 milestone có thể demo được.
