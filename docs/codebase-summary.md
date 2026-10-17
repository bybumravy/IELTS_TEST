# Codebase Summary: SWP_SE1934_Group3 (IELTS Testing Platform)

> **Cập nhật lần cuối:** 2026-10-17  
> **Dự án:** Hệ thống Luyện thi và Đánh giá IELTS trực tuyến (IELTS Prep & Mock Test Platform)  
> **Nhóm thực hiện:** SWP_SE1934_Group3

---

## 1. Tổng quan hệ thống (High-Level Overview)

Dự án là nền tảng luyện thi IELTS tích hợp công nghệ AI và chấm điểm thực tế từ giáo viên, bao gồm đầy đủ 4 kỹ năng: **Listening, Reading, Writing, Speaking** cùng các tính năng hỗ trợ học tập (Mini game từ vựng, mẹo thi, thanh toán gói Premium).

Hệ thống được tổ chức theo mô hình Client-Server phân tách:
- **Frontend (`ReactFrontEnd/`):** Ứng dụng Single Page Application (SPA) phát triển bằng React 18, TypeScript, Vite và TailwindCSS.
- **Backend (`backend/ielts/`):** REST API Server phát triển bằng Java Spring Boot 3.5, lưu trữ trên MongoDB, tích hợp đa dạng dịch vụ đám mây (OpenAI, Azure Speech, AWS S3, VNPay, Google OAuth2).

---

## 2. Kiến trúc & Công nghệ (Tech Stack)

### 2.1. Frontend (`ReactFrontEnd/`)
| Thành phần | Công nghệ / Thư viện | Mô tả |
| :--- | :--- | :--- |
| **Core Framework** | React 18, TypeScript | UI rendering & component logic |
| **Build Tool** | Vite 6 | Fast HMR & build bundling |
| **Styling** | TailwindCSS v4, Radix UI, Ant Design (Antd) | Giao diện hiện đại, responsive, UI kit phong phú |
| **Routing** | React Router DOM v7 | Điều hướng trang & phân quyền route theo role |
| **State Management** | React Context API | `AuthContext` quản lý session đăng nhập |
| **Data Fetching** | Axios | Tương tác RESTful API |
| **Form & Validation** | React Hook Form | Quản lý form đăng ký, đăng nhập, nộp bài |
| **Animation & Charts** | Framer Motion, Recharts | Hiệu ứng UI và biểu đồ thống kê điểm thi |

### 2.2. Backend (`backend/ielts/`)
| Thành phần | Công nghệ / Dịch vụ | Mô tả |
| :--- | :--- | :--- |
| **Framework** | Spring Boot 3.5.0, Java 17 | Core web service & dependency injection |
| **Database** | MongoDB Atlas (`spring-boot-starter-data-mongodb`) | Lưu trữ NoSQL linh hoạt cho bài thi, kết quả, user |
| **Security & Auth** | Spring Security, JWT, Google OAuth2 | Xác thực người dùng, phân quyền RBAC |
| **Speech Processing** | Azure Cognitive Speech + TarsosDSP | Phân tích phát âm, ngữ điệu và chấm điểm Speaking |
| **AI Evaluation** | OpenAI API | Chấm điểm tự động cho Writing & Speaking feedback |
| **Cloud Storage** | AWS S3 (`ap-southeast-1`) | Lưu trữ file audio, bài nghe, đề thi, avatar |
| **Payment Gateway** | VNPay Sandbox API | Thanh toán trực tuyến gói nâng cấp Premium |
| **Mail Service** | Spring Boot Starter Mail (Gmail SMTP) | Gửi OTP kích hoạt tài khoản và reset mật khẩu |

---

## 3. Cấu trúc thư mục chi tiết (Directory Layout)

```text
SWP_SE1934_Group3/
├── docs/                               # Tài liệu dự án (PDR, kiến trúc, summary)
├── plans/                              # Kế hoạch phát triển theo phase
├── backend/
│   └── ielts/                          # Dự án Spring Boot Maven
│       ├── pom.xml                     # Cấu hình dependency backend
│       └── src/main/java/web/ielts/
│           ├── Admin/                  # Quản trị hệ thống (user, đề thi, thống kê)
│           ├── Auth/                   # Đăng ký, đăng nhập, JWT, OAuth2, OTP
│           ├── Config/                 # Cấu hình Security, CORS, S3, Mongo
│           ├── FileUpload/             # Xử lý upload media lên AWS S3
│           ├── History/                # Lịch sử làm bài và tiến độ học viên
│           ├── Payment/                # Tích hợp cổng thanh toán VNPay
│           ├── Practice/               # Luyện tập kỹ năng nhỏ & từ vựng
│           ├── Report/                 # Báo cáo sai sót đề thi, khiếu nại
│           ├── Student/                # Dashboard và thông tin cá nhân học viên
│           ├── Test/                   # Quản lý đề thi & chấm thi 4 kỹ năng (AI & Manual)
│           ├── Tips/                   # Quản lý mẹo và chiến lược thi IELTS
│           └── User/                   # Quản lý tài khoản và phân quyền
└── ReactFrontEnd/                      # Dự án React TypeScript Vite
    ├── package.json                    # Cấu hình dependency frontend
    ├── vite.config.ts                  # Cấu hình Vite & Tailwind
    └── src/
        ├── App.tsx                     # Định tuyến toàn bộ ứng dụng (Router)
        ├── assets/                     # Hình ảnh, biểu tượng, logo
        ├── components/                 # Các UI component dùng chung
        │   ├── layout/                 # Layout: Main, Admin, Staff, DoTest
        │   ├── sections/               # Component nghiệp vụ (Login, Register, AddTest...)
        │   └── history/                # Component lịch sử làm bài
        ├── contexts/                   # Global state (AuthContext)
        ├── hooks/                      # Custom React hooks
        ├── pages/                      # Các trang theo chức năng
        │   ├── DoTest/                 # Giao diện làm bài: Listening, Reading, Writing, Speaking, FullTest
        │   ├── Result/                 # Giao diện xem kết quả và nhận xét chấm điểm
        │   ├── Payment/                # Trang nạp gói Premium & kết quả VNPay
        │   ├── student/                # Dashboard học viên, Vocabulary Mini Game
        │   ├── AdminPage.tsx           # Trang quản trị dành cho Admin
        │   └── StaffPage.tsx           # Trang duyệt đề, quản lý dành cho Staff
        ├── services/                   # API call layer kết nối Backend
        └── types/                      # TypeScript interface/type definitions
```

---

## 4. Các luồng nghiệp vụ chính (Key Workflows)

### 4.1. Luồng Xác thực & Phân quyền (Authentication & Authorization)
- Đăng ký qua email có gửi OTP xác thực (`/api/auth/register` + `/api/auth/verify-email`).
- Đăng nhập bằng tài khoản nội bộ (JWT Token) hoặc Google OAuth2.
- Hệ thống hỗ trợ 4 vai trò chính:
  1. **Student:** Luyện thi, chơi game từ vựng, nộp bài, xem lịch sử, thanh toán gói VIP.
  2. **Teacher:** Chấm điểm bài thi Writing & Speaking, nhận xét chi tiết.
  3. **Staff:** Thêm mới, chỉnh sửa đề thi 4 kỹ năng, kiểm duyệt nội dung.
  4. **Admin:** Quản trị toàn bộ người dùng, quản lý giao dịch thanh toán, cấu hình hệ thống.

### 4.2. Luồng Làm bài & Chấm điểm IELTS (Testing & Scoring Pipeline)
1. **Listening & Reading:** Học viên làm trắc nghiệm/điền từ $\rightarrow$ Backend tự động đối chiếu đáp án $\rightarrow$ Trả kết quả và band score ngay lập tức.
2. **Writing:** Học viên nộp bài văn $\rightarrow$ AI (OpenAI API) phân tích theo 4 tiêu chí IELTS (Task Achievement, Coherence & Cohesion, Lexical Resource, Grammatical Range) hoặc chuyển sang hàng đợi giáo viên chấm.
3. **Speaking:** Học viên ghi âm câu trả lời $\rightarrow$ File gửi lên AWS S3 $\rightarrow$ Azure Cognitive Speech chuyển thành văn bản và đánh giá phát âm/ngữ điệu $\rightarrow$ AI tổng hợp band score.

### 4.3. Luồng Thanh toán VNPay (Payment Gateway Flow)
1. Học viên chọn gói Premium trên `PremiumPage`.
2. Frontend gọi API backend để tạo link redirect đến cổng VNPay Sandbox.
3. Học viên thanh toán xong, VNPay gọi return URL (`VNPayResultPage`) và cập nhật trạng thái gói Premium cho tài khoản.

---

## 5. Hướng dẫn khởi chạy dự án cục bộ (Local Setup & Run)

### 5.1. Khởi chạy Backend (`backend/ielts`)
- **Yêu cầu:** JDK 17+, Maven 3.8+
- **Lệnh chạy:**
  ```powershell
  cd backend\ielts
  .\mvnw.cmd spring-boot:run
  ```
- **Port mặc định:** `8080`

### 5.2. Khởi chạy Frontend (`ReactFrontEnd`)
- **Yêu cầu:** Node.js 18+ (hoặc 20+), npm
- **Lệnh chạy:**
  ```powershell
  cd ReactFrontEnd
  npm install
  npm run dev
  ```
- **Địa chỉ mặc định:** `http://localhost:5173`

---

## 6. Ghi chú bảo mật & Cải tiến cần làm (Security & Tech Debt)

1. **Bảo mật file cấu hình:** `backend/ielts/src/main/resources/application.properties` hiện đang chứa hardcoded credentials (OpenAI key, AWS keys, Azure Speech key, VNPay secret, MongoDB connection URI, Mail password). **Cần chuyển toàn bộ sang Environment Variables hoặc `.env`** trước khi đưa lên Git công khai.
2. **Dọn dẹp Git:** Thư mục gốc cần bổ sung file `.gitignore` để bỏ qua `node_modules`, `target`, `.idea`, file `.env` tránh làm phình to repo.
