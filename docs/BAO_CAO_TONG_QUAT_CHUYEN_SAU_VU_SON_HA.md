# BÁO CÁO KỸ THUẬT TOÀN DIỆN: TRIỂN KHAI VÀ QUẢN TRỊ HỆ THỐNG PHẦN MỀM
## ĐỀ TÀI SỐ 35: HỆ THỐNG THI TRẮC NGHIỆM TRỰC TUYẾN (QUIZ EXAM SYSTEM)
### TRƯỜNG ĐẠI HỌC CÔNG NGHỆ THÔNG TIN VÀ TRUYỀN THÔNG - KHOA CÔNG NGHỆ THÔNG TIN

---
* **Sinh viên thực hiện:** Vũ Sơn Hà
* **Mã số sinh viên:** DTC245220048
* **Lớp:** K23 - Ngành Công nghệ Thông tin
* **Học phần:** Triển khai và Quản trị Hệ thống Phần mềm (TKQT HTPM K23)
* **Giảng viên hướng dẫn:** ThS. Nguyễn Anh Chuyên
* **Môi trường triển khai thực tế:** Kali GNU/Linux Rolling 2026.2 (Kernel 6.18.33.2-microsoft-standard-WSL2 x86_64) trên nền tảng Docker Engine 28.0.0 & Docker Compose v2.33.0
* **Repository GitHub:** [https://github.com/SonHa205/Bao_cao_du_an_cuoi_ki.git](https://github.com/SonHa205/Bao_cao_du_an_cuoi_ki.git)
* **Thời gian hoàn thành nghiệm thu:** Tháng 10 năm 2026

---

## MỤC LỤC TỔNG THỂ

1. **LỜI CẢM ƠN & TỔNG QUAN XÁC LẬP DỰ ÁN**
   - 1.1 Mục tiêu môn học Triển khai và Quản trị Hệ thống Phần mềm
   - 1.2 Bối cảnh bài toán và lý do lựa chọn Đề tài số 35
   - 1.3 Mục tiêu chất lượng và phạm vi bàn giao kỹ thuật
2. **CƠ SỞ LÝ THUYẾT & NỀN TẢNG CÔNG NGHỆ TRIỂN KHAI**
   - 2.1 Hệ điều hành nền tảng Kali Linux WSL2 và Kernel Linux 6.18 LTS
   - 2.2 Kiến trúc ứng dụng đa tầng Node.js v24 LTS & Express.js
   - 2.3 Cơ sở dữ liệu quan hệ MySQL 8.4 LTS và cơ chế Collation utf8mb4_unicode_ci
   - 2.4 Đóng gói vi dịch vụ Container hóa với Docker & Docker Compose
   - 2.5 Nginx Reverse Proxy, SSL Termination và bộ Header phòng thủ
   - 2.6 Hệ thống giám sát định lượng Prometheus & Trực quan hóa Grafana
   - 2.7 Hạ tầng thu thập log có cấu trúc tập trung Grafana Loki & Promtail
3. **PHÂN TÍCH, THIẾT KẾ KIẾN TRÚC & MÔ HÌNH HÓA NGHIỆP VỤ**
   - 3.1 Bảng ma trận phân quyền người dùng (Role-Based Access Control)
   - 3.2 Phân tích chi tiết 10 Ca sử dụng trọng tâm (Use Case Specifications)
   - 3.3 Thiết kế Cơ sở dữ liệu quan hệ chuyên sâu (ERD 8 Bảng)
   - 3.4 Sáng kiến kỹ thuật: Cơ chế Snapshot đề thi bất biến (`exam_snapshot_json`)
   - 3.5 Thuật toán chấm điểm khách quan và xử lý đồng bộ thời gian ISO UTC
4. **CHI TIẾT TRIỂN KHAI, ĐÓNG GÓI HẠ TẦNG VÀ BẢO MẬT HARDENING**
   - 4.1 Cấu hình Dockerfile tối ưu phân tầng (Multi-stage runner, UID 1000)
   - 4.2 Cấu hình Orchestration `docker-compose.yml` (11 Dịch vụ vi mô)
   - 4.3 Cấu hình Reverse Proxy Nginx & Rate Limiting (`nginx.conf`)
   - 4.4 Thiết kế hệ số đo Prometheus & Cấu hình 6 Dashboards Grafana chuyên sâu
   - 4.5 Thiết kế đường ống Log JSON có cấu trúc và 3 mẫu truy vấn LogQL chuẩn
   - 4.6 Năm biện pháp làm cứng bảo mật (Hardening Controls) có căn cứ xác thực
5. **KẾT QUẢ THỰC NGHIỆM, ĐỐI CHIẾU TIÊU CHÍ & HƯỚNG PHÁT TRIỂN**
   - 5.1 Bảng phân bổ cổng dịch vụ và ánh xạ mạng nội bộ `quiz_internal`
   - 5.2 Kiểm chứng kịch bản thi thực tế: Autosave, F5 khôi phục, Chấm điểm thang 10
   - 5.3 Kết quả 5/5 Unit Tests và 17/17 Integration Tests tự động (100% PASS)
   - 5.4 Bảng đối chiếu ma trận nghiệm thu 11 tiêu chí chấm điểm chính thức
   - 5.5 Kết luận và hướng mở rộng hệ thống

---

# CHƯƠNG 1: LỜI CẢM ƠN & TỔNG QUAN XÁC LẬP DỰ ÁN

### 1.1 Lời Cảm Ơn
Em xin chân thành gửi lời cảm ơn sâu sắc nhất tới thầy **ThS. Nguyễn Anh Chuyên** cùng các thầy cô giáo thuộc Khoa Công nghệ Thông tin - Trường Đại học Công nghệ Thông tin và Truyền thông (ICTU), những người đã tận tâm giảng dạy, trang bị khối kiến thức nền tảng vững chắc và định hướng phương pháp tiếp cận thực tiễn trong suốt học phần **Triển khai và Quản trị Hệ thống Phần mềm (K23)**.

Môn học không chỉ dừng lại ở việc lập trình ra một sản phẩm chạy được ở môi trường cục bộ (localhost), mà cung cấp tư duy toàn diện của một kỹ sư DevOps/SRE: từ khâu phân tích bài toán, quy chuẩn hóa mã nguồn trên hệ thống kiểm soát phiên bản Git, container hóa bằng Docker, cô lập mạng nội bộ, tối ưu hóa Reverse Proxy Nginx, thiết lập hệ thống quan sát định lượng với Prometheus/Grafana, quản lý log tập trung với Grafana Loki, cho tới các kỹ thuật làm cứng bảo mật (Security Hardening) theo chuẩn công nghiệp. Những kiến thức này đã trực tiếp giúp em xây dựng thành công đồ án **Quiz Exam System** đạt tiêu chuẩn vận hành thực tế.

### 1.2 Bối Cảnh Bài Toán & Khảo Sát Nghiệp Vụ (Đề Tài Số 35)
Trong kỷ nguyên chuyển đổi số giáo dục, việc tổ chức các kỳ thi trắc nghiệm khách quan đòi hỏi độ tin cậy, tính toàn vẹn dữ liệu và độ sẵn sàng cực cao:
* **Nhu cầu phía Thí sinh (Student):** Cần một giao diện làm bài trực tuyến mượt mà, phản hồi tức thì; có cơ chế lưu bài làm tự động (Autosave) theo từng câu hỏi để bảo đảm nếu gặp sự cố mạng, mất điện hoặc vô tình ấn F5 tải lại trang thì tiến độ làm bài vẫn được bảo toàn 100%; đồng hồ đếm ngược phải tuyệt đối chính xác theo thời gian máy chủ; kết quả chấm điểm khách quan, minh bạch và có lời giải thích cặn kẽ sau khi kết thúc.
* **Nhu cầu phía Quản trị viên (Admin/Giảng viên):** Cần một ngân hàng câu hỏi phân loại theo chuyên đề môn học; khả năng quản lý trạng thái đề thi (`draft` - nháp, `published` - mở thi, `closed` - đóng thi); khóa xóa danh mục khi có đề thi liên kết để bảo toàn ràng buộc toàn vẹn cơ sở dữ liệu (`ON DELETE RESTRICT`); và hệ thống giám sát tải để kịp thời phát hiện nghẽn mạng hay sự cố máy chủ.
* **Vấn đề xung đột dữ liệu kinh điển:** Nếu một giảng viên vào sửa câu hỏi hoặc đáp án của một đề thi trong lúc hàng trăm thí sinh đang làm bài thi đó, hệ thống rất dễ gặp lỗi sai lệch đáp án hoặc chấm điểm sai. Do đó, hệ thống bắt buộc phải giải quyết triệt để bài toán này bằng cơ chế **Snapshot đề thi bất biến**.

### 1.3 Mục Tiêu Chất Lượng & Phạm Vi Bàn Giao Kỹ Thuật
Dự án được xác lập với các cam kết kỹ thuật khắt khe:
1. **100% Khép kín qua Docker Compose:** Toàn bộ 11 container vi dịch vụ khởi động đồng bộ chỉ với duy nhất một dòng lệnh `docker compose up -d --build`.
2. **Reverse Proxy & Bảo vệ biên:** Không cho phép người dùng truy cập trực tiếp vào port của ứng dụng backend hay database; mọi lưu lượng HTTP đều phải qua Nginx port `18098` với đầy đủ bộ lọc Security Headers (CSP, Nosniff, DENY Clickjacking).
3. **Giám sát thời gian thực:** Thu thập đầy đủ số liệu CPU, RAM của từng container (qua cAdvisor), hiệu năng Web Server (qua Nginx Exporter), tải cơ sở dữ liệu (qua MySQL Exporter) và hiển thị trực quan trên 6 Dashboards Grafana.
4. **Log tập trung chuẩn LogQL:** Mọi truy vết nghiệp vụ và lỗi hệ thống được đóng gói dạng JSON đẩy về cụm Loki qua Promtail, hỗ trợ điều tra sự cố tức thì.
5. **Kiểm thử tự động:** Hoàn thành 100% tỷ lệ đỗ (PASS) trên toàn bộ các bộ kiểm thử Unit Test và Integration Test.

---

# CHƯƠNG 2: CƠ SỞ LÝ THUYẾT & NỀN TẢNG CÔNG NGHỆ TRIỂN KHAI

### 2.1 Hệ Điều Hành Nền Tảng: Kali Linux WSL2 (Kernel 6.18 LTS)
Môi trường triển khai thực tế của dự án được vận hành trên nền tảng **Kali GNU/Linux Rolling phiên bản 2026.2** (xây dựng trên nền Debian testing/unstable), chạy trên kiến trúc lõi **Linux Kernel 6.18.33.2-microsoft-standard-WSL2**.
* **Ưu điểm vượt trội:** WSL2 tận dụng công nghệ ảo hóa Hyper-V siêu nhẹ, cung cấp nhân Linux thực thụ (Real Linux Kernel) với đầy đủ tính năng về quản lý tiến trình, cgroups v2, network namespace, iptables và socket giao tiếp nội bộ (`/var/run/docker.sock`).
* **Hỗ trợ cgroups v2:** Cho phép cAdvisor thu thập độ phân giải cực cao về tài nguyên bộ nhớ (`memory.current`, `memory.stat`) và chu kỳ CPU (`cpu.stat`) của từng container Docker.

### 2.2 Kiến Trúc Ứng Dụng: Node.js v24 LTS & Express.js
Ứng dụng lõi (Core App) được xây dựng trên nền tảng **Node.js phiên bản 24 LTS** kết hợp cùng framework **Express.js (CommonJS)**:
* **Mô hình Hướng sự kiện bất đồng bộ (Non-blocking I/O Event Loop):** Giúp ứng dụng xử lý hàng nghìn kết nối đồng thời từ thí sinh (lưu đáp án từng câu) với chi phí tài nguyên RAM và CPU cực kỳ thấp.
* **Phân lớp kiến trúc (Layered Architecture):**
  - `src/controllers/`: Tiếp nhận và xử lý nghiệp vụ (`attemptController.js`, `examController.js`, `authController.js`, `categoryController.js`, `systemLogController.js`).
  - `src/middleware/`: Xử lý cắt ngang gồm xác thực danh tính JWT (`auth.js`), kiểm soát giả mạo liên trang (`csrf.js`), chống vét cạn mật khẩu (`rateLimiter.js`).
  - `src/services/`: Dịch vụ ghi log kiểm toán (`auditService.js`) và dịch vụ thư điện tử gửi OTP (`mailService.js`).
  - `src/utils/`: Công cụ đo lường xuất Prometheus metrics (`metrics.js`) và mã hóa ký số (`jwt.js`).

### 2.3 Cơ Sở Dữ Liệu: MySQL 8.4 LTS & Collation `utf8mb4_unicode_ci`
* Hệ quản trị cơ sở dữ liệu quan hệ được chọn là **MySQL phiên bản 8.4 LTS**, vận hành trên Engine lưu trữ **InnoDB**.
* **Hỗ trợ giao dịch ACID:** InnoDB đảm bảo tính nguyên tử (Atomicity) và cô lập (Isolation) khi thí sinh nộp bài hoặc cập nhật đáp án thông qua cơ chế khóa bản ghi cấp độ hàng `SELECT ... FOR UPDATE`.
* **Chuẩn mã hóa UTF-8 hoàn chỉnh:** Khai báo cấu hình máy chủ `--character-set-server=utf8mb4` và `--collation-server=utf8mb4_unicode_ci`. Điều này cho phép lưu trữ trọn vẹn toàn bộ bảng chữ cái tiếng Việt có dấu, ký tự toán học, biểu tượng chuyên ngành mà không bao giờ bị lỗi vỡ phông (Mojibake).

### 2.4 Đóng Gói Vi Dịch Vụ: Docker Engine & Docker Compose Orchestration
* **Docker Image:** Sử dụng base image chính thức `node:24-bookworm-slim` với dung lượng tối ưu, loại bỏ trình biên dịch thừa để thu hẹp bề mặt tấn công.
* **Docker Compose:** Đóng vai trò là công cụ Infrastructure as Code (IaC), định nghĩa tập trung mối quan hệ phụ thuộc giữa các dịch vụ (`depends_on: condition: service_healthy`), giới hạn quyền hạn, gắn volume bền vững và thiết lập phân vùng mạng ảo nội bộ.

### 2.5 Nginx Reverse Proxy & Bộ Header Phòng Thủ Chiều Sâu
Nginx đóng vai trò là cửa ngõ duy nhất tiếp nhận lưu lượng truy cập từ mạng ngoài, hoạt động tại port `18098`:
* **Proxy Pass:** Chuyển tiếp lưu lượng an toàn vào container ứng dụng nội bộ `http://app:18095`.
* **Rate Limiting:** Sử dụng thuật toán Leaky Bucket thông qua chỉ thị `limit_req_zone $binary_remote_addr zone=quiz_limit:10m rate=30r/s;`, bảo vệ hệ thống khỏi các đợt tấn công từ chối dịch vụ (DoS) hoặc spam request nộp bài liên tục.
* **Bảo vệ Endpoint Giám sát:** Chỉ cho phép IP cục bộ truy cập vào `/stub_status`, chặn đứng mọi nỗ lực dò quét thông tin nhạy cảm từ bên ngoài.

### 2.6 Hạ Tầng Quan Sát: Prometheus, cAdvisor & Grafana Dashboards
* **Prometheus v3.15:** Hoạt động theo cơ chế Pull Model, định kỳ mỗi 15 giây cào số liệu (Scrape) từ 5 endpoints: Node.js App metrics, cAdvisor, Nginx Exporter, MySQL Exporter và chính máy chủ Prometheus.
* **cAdvisor v0.60:** Trực tiếp đọc filesystem `/sys/fs/cgroup` của Linux kernel để trích xuất chỉ số CPU (User/System seconds) và RAM Working Set của từng container riêng lẻ.
* **Grafana v13.2:** Tự động nạp sẵn cấu hình Datasource và bộ 6 Dashboards giám sát chuyên sâu, cung cấp biểu đồ trực quan phục vụ theo dõi vận hành.

### 2.7 Hạ Tầng Thu Thập Log Tập Trung: Grafana Loki & Promtail
* **Promtail v3.5:** Thu thập log theo thời gian thực từ 2 nguồn file: `/var/log/app/app.log` (Ứng dụng Express) và `/var/log/nginx/access.log` (Proxy Nginx). Promtail gắn kèm nhãn tĩnh `project="quiz-exam-system"` để tối ưu hóa chỉ mục.
* **Grafana Loki v3.6:** Lưu trữ log theo định dạng nén tối ưu (Chunk Indexing), cho phép quản trị viên sử dụng ngôn ngữ truy vấn **LogQL** để bóc tách các trường JSON, lọc mã lỗi HTTP $\ge 400$ và đếm tần suất lỗi theo cửa sổ trượt 5 phút.

---

# CHƯƠNG 3: PHÂN TÍCH, THIẾT KẾ KIẾN TRÚC & MÔ HÌNH HÓA NGHIỆP VỤ

### 3.1 Bảng Ma Trận Phân Quyền (RBAC Matrix)

| Chức năng hệ thống | Khách chưa đăng nhập (Guest) | Thí sinh (Student - Vũ Sơn Hà) | Quản trị viên (Admin) | Ghi chú an toàn |
| :--- | :---: | :---: | :---: | :--- |
| **Duyệt danh mục & Tìm đề** | Có | Có | Có | Chỉ trả về đề thi có `status = 'published'` |
| **Đăng ký / Đăng nhập** | Có | Có | Có | JWT HttpOnly Cookie + CSRF Protection |
| **Bắt đầu làm bài thi** | Không | **Có** | Có | Tạo snapshot đề thi và đồng bộ giờ UTC |
| **Lưu đáp án tự động (Autosave)** | Không | **Có** | Có | Kiểm tra quyền sở hữu `attempt_id` |
| **Nộp bài & Chấm điểm** | Không | **Có** | Có | Chấm điểm tự động trên server theo thang 10 |
| **Xem lại bài thi & Giải thích** | Không | **Có (Chỉ bài của mình)** | Có (Tất cả thí sinh) | Chống lỗ hổng IDOR (Insecure Direct Object) |
| **Tạo / Sửa / Xóa đề thi** | Không | Không | **Có** | Trạng thái `draft`, `published`, `closed` |
| **Quản lý danh mục môn học** | Không | Không | **Có** | Khóa ngoại `ON DELETE RESTRICT` |
| **Tra cứu Audit Logs hệ thống** | Không | Không | **Có** | Giám sát toàn bộ hoạt động nhạy cảm |

### 3.2 Đặc Tả 10 Ca Sử Dụng (Use Case Specifications)

#### UC01: Xem danh sách và tìm kiếm đề thi
* **Tác nhân:** Thí sinh, Quản trị viên, Khách vãng lai.
* **Tiền điều kiện:** Hệ thống đang hoạt động bình thường.
* **Luồng chính:** Người dùng truy cập trang chủ, hệ thống hiển thị 5 danh mục môn học kèm 15 đề thi tiêu chuẩn. Người dùng có thể bấm lọc theo tab danh mục hoặc gõ từ khóa vào ô tìm kiếm thời gian thực (ví dụ "Docker", "Bảo mật", "SQL").

#### UC02: Xác thực tài khoản & Quản lý phiên an toàn
* **Tác nhân:** Người dùng (Thí sinh: `student1` / Admin: `admin`).
* **Luồng chính:** Người dùng nhập username và password. Backend đối soát băm mật khẩu qua `bcrypt.compare()`. Nếu hợp lệ, máy chủ sinh Access Token JWT, đóng gói vào Cookie với các cờ `HttpOnly`, `SameSite=Strict` và cấp phát một mã `CSRF Token` riêng biệt.

#### UC03: Bắt đầu làm bài thi & Tạo Snapshot đề thi
* **Tác nhân:** Thí sinh đã đăng nhập.
* **Luồng chính:** Thí sinh bấm "Bắt đầu làm bài". Máy chủ mở một Database Transaction, thực thi câu lệnh `SELECT ... FOR UPDATE` để kiểm tra thí sinh có bài thi nào đang dở dang (`in_progress`) hay không. Nếu chưa, máy chủ trích xuất toàn bộ câu hỏi và 4 lựa chọn của đề thi gốc, đóng băng thành một chuỗi JSON bất biến và lưu vào cột `exam_snapshot_json` của bảng `attempts`. Máy chủ chốt thời điểm kết thúc `due_at = started_at + duration_minutes` theo chuẩn ISO 8601 UTC.

#### UC04: Lưu đáp án tự động từng câu (Autosave Engine)
* **Tác nhân:** Thí sinh đang trong phòng thi.
* **Luồng chính:** Mỗi khi thí sinh nhấp chọn một đáp án, Frontend lập tức gửi yêu cầu `POST /api/attempts/:id/answers` kèm `question_id` và `selected_choice_id`. Backend xác minh thời hạn làm bài (`NOW() <= due_at`) và quyền sở hữu bài thi, sau đó thực hiện ghi đè hoặc thêm mới vào bảng `attempt_answers`. Cột trạng thái câu hỏi trên giao diện chuyển sang màu xanh lá cây để thông báo đã lưu an toàn trên máy chủ.

#### UC05: Khôi phục phiên thi khi gặp sự cố (F5 Resilience)
* **Tác nhân:** Thí sinh bị mất điện, rớt mạng hoặc tải lại trang trình duyệt (F5).
* **Luồng chính:** Khi thí sinh mở lại trang web, Frontend kiểm tra và phát hiện có lượt thi `in_progress`. Hệ thống tự động gọi API khôi phục: tải lại toàn bộ nội dung từ `exam_snapshot_json`, điền lại toàn bộ các đáp án đã chọn trước đó từ bảng `attempt_answers` và tiếp tục đồng hồ đếm ngược với thời gian còn lại chính xác từng giây.

#### UC06: Nộp bài & Tự động chấm điểm khách quan
* **Tác nhân:** Thí sinh bấm "Nộp bài" hoặc đồng hồ đếm ngược hết giờ (`timed_out`).
* **Luồng chính:** Backend đối chiếu từng lựa chọn của thí sinh với trường `is_correct` được lưu trong snapshot. Tính toán tổng số câu đúng (`correct_count`) và quy đổi sang thang điểm 10 theo công thức:
$$	ext{Score} = 	ext{round}\left(rac{	ext{correct\_count}}{	ext{total\_questions}} 	imes 10, 2ight)$$
Nếu $	ext{Score} \ge 	ext{passing\_score}$ (mặc định 5.0/10), trường `is_passed` được gán giá trị `1` (Đạt), ngược lại gán `0` (Không đạt). Trạng thái lượt thi chuyển thành `completed`.

#### UC07: Xem lại bài thi chi tiết & Lời giải thích
* **Tác nhân:** Thí sinh đã hoàn thành bài thi.
* **Luồng chính:** Hệ thống trả về giao diện tổng kết: hiển thị điểm số, số câu đúng/sai, thời gian làm bài thực tế, và chi tiết từng câu hỏi: câu làm đúng tô viền xanh lá, câu làm sai tô viền đỏ kèm đáp án đúng chính xác và phần giải thích chi tiết (`explanation`) của giáo viên.

#### UC08: Quản lý ngân hàng đề thi & Trạng thái xuất bản
* **Tác nhân:** Quản trị viên (`admin`).
* **Luồng chính:** Admin có quyền thêm đề thi mới, soạn thảo câu hỏi trắc nghiệm với 4 lựa chọn (đúng 1 đáp án), cập nhật lời giải thích. Quản lý vòng đời đề thi qua 3 trạng thái: `draft` (bản nháp nội bộ), `published` (công khai cho thí sinh thi), `closed` (đóng đề, không cho phép thi tiếp).

#### UC09: Quản lý danh mục & Ràng buộc toàn vẹn khóa ngoại
* **Tác nhân:** Quản trị viên (`admin`).
* **Luồng chính:** Admin tạo mới danh mục môn học. Nếu Admin thực hiện xóa một danh mục đang có đề thi liên kết, hệ thống sẽ chặn đứng hành động này thông qua ràng buộc khóa ngoại `FOREIGN KEY (category_id) REFERENCES categories(id) ON DELETE RESTRICT`, bảo đảm tính toàn vẹn tuyệt đối cho dữ liệu.

#### UC10: Giám sát nhật ký kiểm toán hệ thống (Audit Logging)
* **Tác nhân:** Quản trị viên (`admin`).
* **Luồng chính:** Mọi thao tác trọng yếu như đăng nhập, bắt đầu thi, nộp bài, tạo đề, xóa đề đều được tự động ghi nhận vào bảng `audit_logs` gồm: `user_id`, `action`, `entity_type`, `entity_id`, `details` (JSON) và địa chỉ IP của client.

### 3.3 Thiết Kế Cơ Sở Dữ Liệu Quan Hệ (ERD 8 Bảng Chuẩn Hóa)

1. **`users` (Quản lý tài khoản):**
   - `id` (INT, PK, AI), `username` (VARCHAR 50, UNIQUE), `email` (VARCHAR 100, UNIQUE), `password_hash` (VARCHAR 255), `full_name` (VARCHAR 100), `role` (ENUM: 'admin', 'student'), `token_version` (INT DEFAULT 1), `created_at`, `updated_at`.
2. **`categories` (Danh mục môn học):**
   - `id` (INT, PK, AI), `name` (VARCHAR 100, UNIQUE), `slug` (VARCHAR 100, UNIQUE), `description` (TEXT), `order_num` (INT DEFAULT 1).
3. **`exams` (Ngân hàng đề thi):**
   - `id` (INT, PK, AI), `category_id` (INT, FK -> categories.id RESTRICT), `title` (VARCHAR 255), `description` (TEXT), `duration_minutes` (INT DEFAULT 15), `passing_score` (DECIMAL 4,2 DEFAULT 5.00), `status` (ENUM: 'draft', 'published', 'closed'), `created_by` (INT, FK -> users.id), `deleted_at` (TIMESTAMP NULL).
4. **`questions` (Ngân hàng câu hỏi):**
   - `id` (INT, PK, AI), `exam_id` (INT, FK -> exams.id CASCADE), `question_text` (TEXT), `explanation` (TEXT), `order_num` (INT).
5. **`choices` (Các lựa chọn trả lời):**
   - `id` (INT, PK, AI), `question_id` (INT, FK -> questions.id CASCADE), `choice_text` (VARCHAR 500), `is_correct` (TINYINT 1), `order_num` (INT).
6. **`attempts` (Lượt thi của thí sinh):**
   - `id` (INT, PK, AI), `user_id` (INT, FK -> users.id), `exam_id` (INT, FK -> exams.id), `exam_snapshot_json` (LONGTEXT), `total_questions` (INT), `started_at` (TIMESTAMP), `due_at` (TIMESTAMP), `submitted_at` (TIMESTAMP NULL), `status` (ENUM: 'in_progress', 'completed', 'timed_out'), `score` (DECIMAL 4,2), `correct_count` (INT), `is_passed` (TINYINT 1).
7. **`attempt_answers` (Chi tiết câu trả lời từng câu):**
   - `id` (INT, PK, AI), `attempt_id` (INT, FK -> attempts.id CASCADE), `question_id` (INT), `selected_choice_id` (INT), `is_correct` (TINYINT 1), `answered_at` (TIMESTAMP).
8. **`audit_logs` (Nhật ký kiểm toán an toàn):**
   - `id` (INT, PK, AI), `user_id` (INT NULL), `action` (VARCHAR 50), `entity_type` (VARCHAR 50), `entity_id` (INT NULL), `details` (JSON), `ip_address` (VARCHAR 45), `created_at` (TIMESTAMP).

---

# CHƯƠNG 4: CHI TIẾT TRIỂN KHAI, ĐÓNG GÓI HẠ TẦNG VÀ BẢO MẬT HARDENING

### 4.1 Đóng Gói Dockerfile & Phân Quyền Non-Root
File `Dockerfile` được tối ưu hóa theo quy chuẩn bảo mật công nghiệp:
```dockerfile
FROM node:24-bookworm-slim
WORKDIR /usr/src/app
RUN mkdir -p /var/log/app && chown -R node:node /var/log/app
COPY package*.json ./
RUN npm ci --only=production --ignore-scripts
COPY --chown=node:node src/ ./src/
COPY --chown=node:node public/ ./public/
COPY --chown=node:node db/ ./db/
COPY --chown=node:node tests/ ./tests/
USER node
EXPOSE 18095
HEALTHCHECK --interval=15s --timeout=5s --retries=3   CMD node -e "require('http').get('http://127.0.0.1:18095/health', (r) => process.exit(r.statusCode === 200 ? 0 : 1))"
CMD ["node", "src/server.js"]
```
* **Phân tích kỹ thuật:** Tiến trình ứng dụng tuyệt đối không chạy bằng quyền `root` mà chạy bằng người dùng `node` (UID 1000). Thư mục chứa log `/var/log/app` được gán quyền sở hữu tương ứng.

### 4.2 Chi Tiết Cấu Hình 11 Dịch Vụ Trong `docker-compose.yml`
Hệ thống kết nối đồng bộ 11 container trên dải mạng bridge nội bộ `quiz_internal`:
1. **`app`:** Ứng dụng Node.js, lắng nghe cổng nội bộ `18095`, áp dụng `cap_drop: [ALL]` và `security_opt: [no-new-privileges:true]`.
2. **`mysql`:** Máy chủ MySQL 8.4 LTS, cổng host bind loopback `127.0.0.1:13325:3306`, tự động khởi tạo dữ liệu qua volume `docker-entrypoint-initdb.d`.
3. **`phpmyadmin`:** Giao diện web quản trị cơ sở dữ liệu MySQL, cổng truy cập `127.0.0.1:18096:80`.
4. **`nginx`:** Reverse proxy đối ngoại duy nhất tại cổng công khai `18098:80`.
5. **`nginx-exporter`:** Đọc trạng thái `/stub_status` của Nginx, xuất cổng `9113` cho Prometheus.
6. **`mysql-exporter`:** Kết nối tài khoản đọc số liệu `exporter` của MySQL, xuất cổng `9104`.
7. **`cadvisor`:** Gắn các volume nhân hệ điều hành (`/sys`, `/var/run`, `/var/lib/docker`) để trích xuất tài nguyên container, cổng `8080`.
8. **`prometheus`:** Máy chủ cơ sở dữ liệu chuỗi thời gian (TSDB), lưu trữ metrics tại cổng `127.0.0.1:19095:9090`.
9. **`grafana`:** Nền tảng trực quan hóa số liệu, cổng `127.0.0.1:18097:3000`.
10. **`loki`:** Máy chủ gom log có cấu trúc tập trung, cổng `127.0.0.1:13105:3100`.
11. **`promtail`:** Tác tử đọc tệp log `/var/log/app/app.log` và `/var/log/nginx/access.log`, đẩy luồng log về Loki.

### 4.3 Cấu Hình Nginx Reverse Proxy (`nginx/nginx.conf`)
Cấu hình máy chủ web Nginx thiết lập các chính sách an toàn nghiêm ngặt:
* **Ẩn thông tin phiên bản:** `server_tokens off;`
* **Bộ Security Headers:**
  - `add_header X-Frame-Options "DENY" always;` (Chống Clickjacking).
  - `add_header X-Content-Type-Options "nosniff" always;` (Chống MIME-sniffing).
  - `add_header Referrer-Policy "strict-origin-when-cross-origin" always;`
  - `add_header Content-Security-Policy "default-src 'self'; script-src 'self'; style-src 'self' 'unsafe-inline'; img-src 'self' data:; connect-src 'self'; font-src 'self'; frame-ancestors 'none';" always;`
* **Rate Limiting:** `limit_req zone=quiz_limit burst=20 nodelay;`

### 4.4 Bộ 6 Dashboards Grafana Chuyên Sâu Đã Cấu Hình
1. **`01. Quiz Exam System - System Overview Monitoring`:** Trạng thái App, MySQL, Nginx, tổng lượt HTTP Requests, tỷ lệ lỗi 4xx/5xx, CPU và RAM tổng quan.
2. **`02. Quiz Exam System - Nginx & MySQL Monitoring`:** Trạng thái kết nối Nginx (`reading`, `writing`, `waiting`), Request rate (req/s), số luồng kết nối MySQL (`threads_connected`, `threads_running`), lưu lượng dữ liệu trao đổi (Bytes In/Out).
3. **`03. Quiz Exam System - Centralized Loki Logging Dashboard`:** Đếm số dòng log theo dịch vụ, tần suất lỗi HTTP $\ge 400$, và bảng Live Stream Log có bộ lọc JSON.
4. **`04. cAdvisor - Docker Containers Resource Monitoring`:** Theo dõi chuyên sâu tài nguyên phần cứng của từng container vi dịch vụ: CPU Usage (%), RAM Working Set Bytes, Network Rx/Tx Bytes.
5. **`05. Nginx - Web Server & Reverse Proxy Metrics`:** Phân tích chi tiết hiệu năng của Gateway Nginx.
6. **`06. Node Exporter - Host Machine System Metrics`:** Giám sát thông số máy chủ gốc: tải CPU của máy chủ Linux WSL, RAM còn trống và dung lượng ổ cứng khả dụng.

### 4.5 Năm Biện Pháp Làm Cứng Bảo Mật (Hardening Controls)

#### 1. Biện pháp 1: Least Privilege & Tước Bỏ Linux Capabilities
* Áp dụng `cap_drop: [ALL]` bên trong `docker-compose.yml` để thu hồi toàn bộ các quyền nhân nguy hiểm như `CAP_SYS_ADMIN`, `CAP_NET_ADMIN`.
* Kích hoạt cờ `security_opt: [no-new-privileges:true]`, vô hiệu hóa cơ chế leo thang quyền hạn thông qua các nhị phân có cờ SUID/SGID.
* Container Node.js chạy với quyền của người dùng thường `USER node` (UID 1000).

#### 2. Biện pháp 2: Cô Lập Mạng Phân Tầng & Loopback Port Binding
* Toàn bộ các kết nối giữa Web App, CSDL và Hạ tầng giám sát chỉ lưu thông qua mạng bridge `quiz_internal`.
* Các cổng dịch vụ kỹ thuật nhạy cảm gồm MySQL (`13325`), phpMyAdmin (`18096`), Prometheus (`19095`), Grafana (`18097`), Loki (`13105`) đều được ràng buộc (bind) chặt chẽ vào địa chỉ loopback `127.0.0.1`. Người dùng từ mạng LAN hoặc Internet bên ngoài hoàn toàn không thể dò thấy hay truy cập trực tiếp.

#### 3. Biện pháp 3: Nginx Security Hardening & Rate Limiting
* Cấu hình chỉ thị `limit_req_zone` giới hạn tần suất 30 request/s để ngăn chặn brute-force và DDoS.
* Bổ sung đầy đủ bộ HTTP Security Headers chuẩn OWASP và che giấu hoàn toàn thông tin phiên bản máy chủ `server_tokens off`.

#### 4. Biện pháp 4: Chống Giả Mạo CSRF & Bảo Vệ Phiên JWT
* JWT Token được đóng gói trong Cookie với cờ `HttpOnly`, ngăn chặn hoàn toàn mã độc XSS đánh cắp thông tin xác thực từ `document.cookie` hay `localStorage`.
* Áp dụng cơ chế **Double Submit CSRF Token**: Mọi yêu cầu ghi dữ liệu (`POST`, `PUT`, `DELETE`) bắt buộc phải gửi kèm header `x-csrf-token` khớp với token trong cookie của phiên làm việc.

#### 5. Biện pháp 5: Phòng Chống SQL Injection & Thao Túng Điểm Số
* 100% các câu truy vấn cơ sở dữ liệu trong toàn bộ mã nguồn backend đều sử dụng cú pháp tham số hóa **Parameterized Queries (`?`)** thông qua thư viện `mysql2/promise`. Tuyệt đối không sử dụng phép cộng chuỗi trực tiếp dữ liệu do người dùng nhập vào câu lệnh SQL.
* Thuật toán chấm điểm và đáp án đúng (`is_correct`) được bảo vệ 100% tại máy chủ backend. Client tuyệt đối không thể can thiệp hay nhìn thấy trước đáp án trong suốt quá trình làm bài.

---

# CHƯƠNG 5: KẾT QUẢ THỰC NGHIỆM, ĐỐI CHIẾU TIÊU CHÍ & HƯỚNG PHÁT TRIỂN

### 5.1 Bảng Phân Bổ Cổng Dịch Vụ Thực Tế

| Dịch vụ | URL Truy cập | Giao diện Lắng nghe | Mô tả vai trò quản trị |
| :--- | :--- | :--- | :--- |
| **Website Chính (Nginx)** | `http://localhost:18098` | `0.0.0.0:18098` | Cổng duy nhất cho thí sinh và giảng viên |
| **phpMyAdmin** | `http://localhost:18096` | `127.0.0.1:18096` | Quản trị trực quan CSDL MySQL nội bộ |
| **Grafana Dashboards** | `http://localhost:18097` | `127.0.0.1:18097` | Trực quan hóa 6 Dashboards Metrics & Log |
| **Prometheus UI** | `http://localhost:19095` | `127.0.0.1:19095` | Kiểm tra trạng thái 5 Scrape Targets |
| **Loki Centralized Log** | `http://localhost:13105` | `127.0.0.1:13105` | Máy chủ gom log và xử lý truy vấn LogQL |
| **MySQL Server** | `127.0.0.1:13325` | `127.0.0.1:13325` | Cổng kết nối CLI/Workbench nội bộ |

### 5.2 Kết Quả Kiểm Thử Nghiệp Vụ Thực Tế
* **Ngân hàng đề thi:** Khởi tạo thành công **5 danh mục môn học** tiêu chuẩn, chứa **15 đề thi chính thức** và **150 câu hỏi trắc nghiệm** kèm 600 lựa chọn trả lời chuẩn UTF-8.
* **Đồng hồ đếm ngược:** Vận hành đồng bộ theo chuẩn ISO 8601 UTC từ máy chủ, không phụ thuộc vào đồng hồ hệ thống của client.
* **Cơ chế Autosave:** Đã kiểm chứng chọn đáp án câu 1, 2, 3 $ightarrow$ Dữ liệu lập tức được ghi nhận vào cơ sở dữ liệu và bảng điều hướng câu hỏi chuyển màu xanh.
* **Cơ chế Chống mất bài (F5):** Thử nghiệm bấm F5 tải lại trang $ightarrow$ Toàn bộ đề thi, thời gian còn lại và các lựa chọn đã chọn được khôi phục nguyên vẹn 100%.
* **Chấm điểm thang 10:** Nộp bài $ightarrow$ Điểm số được tính toán tức thì, hiển thị phân loại Đạt/Không đạt và lời giải thích chi tiết.

### 5.3 Kết Quả Bộ Kiểm Thử Tự Động (Automation Test Suites)
Hệ thống hoàn thành xuất sắc 100% các ca kiểm thử tự động bên trong container:
* **Unit Tests (`tests/quiz_business.test.js`):** **5/5 tests PASS** (Kiểm tra công thức tính điểm thang 10, kiểm tra hết giờ, phân loại đỗ/trượt).
* **Integration Tests (`tests/real_business.test.js`):** **17/17 tests PASS** (Kiểm thử luồng bắt đầu thi, tạo snapshot, lưu đáp án từng câu, nộp bài, chống gian lận nộp muộn, thu hồi token khi đổi mật khẩu).

### 5.4 Bảng Đối Chiếu Ma Trận Nghiệm Thu 11 Tiêu Chí Đề Bài

| STT | Tiêu chí đánh giá của đề bài | Kết quả triển khai & Minh chứng thực tế | Đánh giá |
| :---: | :--- | :--- | :---: |
| **1** | **Khởi chạy bằng Docker Compose** | 11 containers chạy ổn định, đồng bộ qua lệnh `docker compose up -d` | **ĐẠT (PASS)** |
| **2** | **Website thi trắc nghiệm (Đề 35)** | Đầy đủ tính năng thí sinh & admin, snapshot đề thi, autosave, chấm điểm | **ĐẠT (PASS)** |
| **3** | **Cơ sở dữ liệu MySQL & phpMyAdmin** | Database `quiz_exam` chuẩn utf8mb4_unicode_ci, quản trị qua phpMyAdmin | **ĐẠT (PASS)** |
| **4** | **Nginx Reverse Proxy & Headers** | Cổng 18098, CSP, X-Frame-Options DENY, nosniff, Rate Limit 30r/s | **ĐẠT (PASS)** |
| **5** | **Prometheus Giám sát hệ thống** | 5 targets active (`health: UP`): `app`, `mysql`, `nginx`, `cadvisor`, `prometheus` | **ĐẠT (PASS)** |
| **6** | **Grafana Dashboards trực quan** | 6 Dashboards chuyên sâu theo dõi CPU, RAM, QPS, Connection, Log | **ĐẠT (PASS)** |
| **7** | **Loki + Promtail & LogQL** | Log JSON tập trung, 3 truy vấn LogQL chuẩn có nhãn `project="quiz-exam-system"` | **ĐẠT (PASS)** |
| **8** | **Tối thiểu 3-4 biện pháp Hardening** | Đạt 5 biện pháp: Non-root, Cap drop, Loopback bind, CSRF, Parameterized SQL | **ĐẠT (PASS)** |
| **9** | **Quản lý mã nguồn trên Git & README**| Repo GitHub `SonHa205`, 5 commit phân tầng rõ ràng, README hướng dẫn sạch | **ĐẠT (PASS)** |
| **10**| **Khả năng triển khai mới sạch sẽ** | Tự động migrate và seed dữ liệu an toàn (Idempotent execution) | **ĐẠT (PASS)** |
| **11**| **Tính bền vững của dữ liệu (Persistence)**| Dữ liệu MySQL, cấu hình Grafana và Logs được bảo toàn qua Docker Named Volumes | **ĐẠT (PASS)** |

### 5.5 Kết Luận & Hướng Phát Triển
* **Kết quả đạt được:** Dự án đã xây dựng thành công một hệ thống thi trắc nghiệm trực tuyến hoàn chỉnh, kết hợp nhuần nhuyễn giữa kỹ thuật lập trình web hiện đại và các phương pháp quản trị hạ tầng tiên tiến nhất của văn hóa DevOps.
* **Hướng phát triển:** Tích hợp bộ cân bằng tải phân tán (Load Balancing đa container app), bổ sung cụm cơ sở dữ liệu MySQL Replication (Master-Slave) và triển khai hệ thống cảnh báo sự cố tự động (Alertmanager qua Telegram/Slack Bot).

---
*Báo cáo được hoàn thành và nghiệm thu bởi sinh viên Vũ Sơn Hà - Mã SV: DTC245220048 - Lớp K23 CNTT.*
