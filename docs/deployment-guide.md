# Hướng dẫn Deploy Miễn phí (Zero-Cost Deployment Guide)

Tài liệu hướng dẫn triển khai hệ thống **IELTSTEST** hoàn toàn miễn phí (0 VNĐ / 0 USD) bằng **Vercel** (Frontend) và **Render.com** (Backend).

---

## 1. Triển khai Frontend lên Vercel (Miễn phí 100%)

Vercel cung cấp gói Hobby miễn phí vĩnh viễn, tự động build mỗi khi push code lên GitHub, kèm chứng chỉ HTTPS/SSL và CDN toàn cầu.

### Các bước thực hiện:
1. Đăng nhập vào [vercel.com](https://vercel.com) bằng tài khoản GitHub của bạn.
2. Bấm **"Add New..."** $\rightarrow$ chọn **"Project"**.
3. Tìm và chọn repository của bạn (`IELTSTEST` hoặc repo bạn vừa tạo) $\rightarrow$ bấm **Import**.
4. Cấu hình dự án trên giao diện Vercel:
   - **Project Name:** Đặt tên tùy ý (ví dụ: `ielts-test-web`).
   - **Framework Preset:** Chọn **Vite**.
   - **Root Directory:** Bấm **Edit**, chọn thư mục `ReactFrontEnd`.
   - **Build and Output Settings:** Giữ mặc định (`npm run build`, output directory: `dist`).
   - **Environment Variables:**
     - Thêm biến: `VITE_API_URL`
     - Giá trị: Link backend từ Render (ví dụ: `https://ielts-api.onrender.com`).
5. Bấm **Deploy**.
   - *(Dự án đã có sẵn file `ReactFrontEnd/vercel.json` nên khi F5 lại bất kỳ trang nào như `/test`, `/history` đều sẽ không bị lỗi 404).*

---

## 2. Triển khai Backend lên Render.com (Miễn phí 100%)

Render cung cấp 750 giờ chạy miễn phí/tháng (đủ chạy 24/7 cho 1 service) và có hỗ trợ Docker.

### Các bước thực hiện:
1. Đăng nhập vào [render.com](https://render.com) bằng tài khoản GitHub.
2. Bấm **New +** ở góc trên bên phải $\rightarrow$ chọn **Web Service**.
3. Chọn tùy chọn **"Build and deploy from a Git repository"** $\rightarrow$ Kết nối repo của bạn.
4. Cấu hình Web Service:
   - **Name:** Đặt tên (ví dụ: `ielts-backend`).
   - **Region:** Chọn `Singapore` (để kết nối về Việt Nam nhanh nhất).
   - **Branch:** `main` (hoặc `master`).
   - **Root Directory:** `backend/ielts`
   - **Runtime:** Chọn **Docker** *(Backend đã có sẵn `backend/ielts/Dockerfile` tối ưu nhiều tầng nhẹ và build cực nhanh)*.
   - **Instance Type:** Chọn **Free** ($0/month).
5. **Cấu hình Environment Variables (Quan trọng):**
   Cuộn xuống mục **Environment Variables**, thêm các biến cấu hình:
   - `MONGODB_URI`: Chuỗi kết nối MongoDB Atlas của bạn.
   - `OPENAI_API_KEY`: API key OpenAI.
   - `JWT_SECRET`: Khóa bí mật JWT (chuỗi bất kỳ dài từ 32 ký tự).
   - `AWS_ACCESS_KEY_ID`: Key AWS của bạn.
   - `AWS_SECRET_ACCESS_KEY`: Secret AWS của bạn.
   - `AZURE_SPEECH_KEY`: Key Azure Speech.
   - `MAIL_USERNAME` & `MAIL_PASSWORD`: Gmail SMTP.
   - `VNPAY_SECRET_KEY`: Khóa bí mật VNPay.
   *(Lưu ý: Phần mềm ngữ âm Praat, các Praat script chấm trọng âm/ngữ điệu và từ điển phát âm chuẩn CMU Pronouncing Dict đã được tự động tích hợp sẵn và đóng gói hoàn toàn trong `Dockerfile`, bạn không cần cài thêm thủ công trên server).*
6. Bấm **Create Web Service**.
   - Render sẽ tự động kéo code, build Docker image và khởi động ứng dụng.
   - Khi hoàn tất, Render sẽ cấp URL dạng: `https://ielts-backend.onrender.com`.

---

## 3. Kết nối Frontend và Backend

1. Sau khi Backend trên Render có link (ví dụ: `https://ielts-backend.onrender.com`):
   - Vào Vercel $\rightarrow$ Chọn Project Frontend $\rightarrow$ **Settings** $\rightarrow$ **Environment Variables**.
   - Cập nhật giá trị `VITE_API_URL = https://ielts-backend.onrender.com`.
   - Vào tab **Deployments** $\rightarrow$ bấm **Redeploy** để frontend nhận URL mới.

2. Cập nhật CORS Backend (nếu cần):
   - Mở file `backend/ielts/src/main/java/web/ielts/Config/CorsConfig.java` và `SecurityConfig.java`.
   - Thêm domain của Vercel (ví dụ: `https://ielts-test-web.vercel.app`) vào danh sách `allowedOrigins`.
