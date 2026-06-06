# AI Lead Scoring & Automation - Hệ thống phân loại & Chấm điểm Khách hàng tiềm năng

Dự án này là hệ thống chấm điểm và phân loại khách hàng tiềm năng (Lead Scoring) trong ngành Bất động sản. Hệ thống tự động tải dữ liệu từ Google Sheets (Hỗ trợ cả Sheet **Công khai** và Sheet **Riêng tư / Bảo mật**), phân tích nhu cầu khách hàng theo các tiêu chí nghiệp vụ quy định, sau đó xuất ra file báo cáo Excel được định dạng chuyên nghiệp để bàn giao.

Dự án hỗ trợ chạy giao diện trực quan bằng Web App (Streamlit) hoặc chạy offline nhanh gọn bằng dòng lệnh (Python CLI).

---

## 📁 Cấu Trúc Thư Mục Dự Án

*   `process_leads.py`: Script Python chính chạy bằng dòng lệnh (CLI).
*   `app_lead_scoring.py`: Ứng dụng Web App Streamlit với Dashboard thống kê trực quan.
*   `real_estate_ai_banner.png`: Ảnh banner nhận diện thương hiệu cao cấp cho dashboard.
*   `tieu_chi_cham_diem.txt`: Tài liệu mô tả các tiêu chí nghiệp vụ cộng/trừ điểm và phân loại thô.
*   `lead_scoring_skill.md`: Tài liệu hướng dẫn định dạng Skill AI chi tiết để phát triển hệ thống.
*   `requirements.txt`: Danh sách các thư viện Python phụ thuộc cần thiết.
*   `.gitignore`: File cấu hình Git để bỏ qua các thư mục bộ nhớ đệm và file kết xuất trung gian.
*   `.streamlit/secrets.toml`: File cấu hình thông tin bảo mật cục bộ của Streamlit (Cần tự tạo nếu chạy trên máy tính).

---

## 🔒 Hướng Dẫn Kết Nối Google Sheet Riêng Tư (Private)

Để ứng dụng có thể đọc được dữ liệu khi bạn đặt Google Sheets ở chế độ **Riêng tư (Private)** thay vì để link công khai, vui lòng thực hiện các bước cấu hình bảo mật sau:

### Bước 1: Tạo Google Service Account (Tài khoản dịch vụ)
1. Truy cập vào [Google Cloud Console](https://console.cloud.google.com/).
2. Tạo mới một Project hoặc chọn Project sẵn có của bạn.
3. Vào phần **APIs & Services > Library**, tìm kiếm và nhấn **Enable** cho 2 API sau:
   *   **Google Sheets API**
   *   **Google Drive API**
4. Vào phần **APIs & Services > Credentials** > click **Create Credentials** > chọn **Service Account**.
5. Đặt tên cho tài khoản dịch vụ, sau đó nhấn **Create and Continue** và **Done**.
6. Tại danh sách Service Account vừa tạo, click vào địa chỉ email của nó > chọn tab **Keys** > click **Add Key** > chọn **Create new key** > định dạng **JSON**.
7. File JSON chứa khóa bảo mật sẽ được tải xuống máy tính của bạn.

### Bước 2: Chia sẻ Google Sheet với Service Account
1. Mở file JSON khóa bảo mật vừa tải xuống và tìm trường `"client_email"` (ví dụ: `account-name@project-id.iam.gserviceaccount.com`).
2. Mở file Google Sheet dữ liệu của bạn, nhấn nút **Chia sẻ (Share)** ở góc trên bên phải.
3. Dán địa chỉ email của Service Account trên vào ô chia sẻ, cấp quyền **Người xem (Viewer)** hoặc **Người chỉnh sửa (Editor)**, rồi nhấn **Chia sẻ**.

### Bước 3: Cấu hình khóa bảo mật vào ứng dụng

Bạn có thể lựa chọn 1 trong 2 cách sau để lưu khóa bảo mật:

*   **Cách 1: Lưu trực tiếp bằng file JSON (Nhanh gọn khi chạy máy cá nhân)**:
    *   Đổi tên file JSON vừa tải về ở Bước 1 thành **`google_credentials.json`**.
    *   Đặt file này trực tiếp tại thư mục dự án (`Demo07/google_credentials.json`). Hệ thống sẽ tự nhận diện.

*   **Cách 2: Cấu hình thông qua Streamlit Secrets (Khuyên dùng khi Deploy lên Web)**:
    *   **Khi chạy cục bộ**: Tạo thư mục `.streamlit` trong dự án và tạo file `secrets.toml` bên trong. Mở file và dán nội dung sau:
        ```toml
        [gcp_service_account]
        type = "service_account"
        project_id = "tên-project-của-bạn"
        private_key_id = "..."
        private_key = "-----BEGIN PRIVATE KEY-----\n...\n-----END PRIVATE KEY-----\n"
        client_email = "your-service-account-email@..."
        client_id = "..."
        auth_uri = "https://accounts.google.com/o/oauth2/auth"
        token_uri = "https://oauth2.googleapis.com/token"
        auth_provider_x509_cert_url = "https://www.googleapis.com/oauth2/v1/certs"
        client_x509_cert_url = "..."
        ```
    *   **Khi đẩy lên Streamlit Community Cloud**: Copy toàn bộ nội dung file JSON bảo mật và dán vào mục **Secrets** trong phần cài đặt App (App Settings) trên Dashboard của Streamlit Cloud với cấu trúc TOML tương tự như trên.

---

## 🚀 Hướng Dẫn Cài Đặt & Chạy Ứng Dụng

### Bước 1: Chuẩn bị môi trường Python

Mở Terminal và điều hướng đến thư mục dự án:
```bash
cd "/Users/lyhoangphuc/Documents/[MindX School] PNL- AI4A01 - Google Drive/07. Lesson 7/Demo07"
```

Khởi tạo và kích hoạt môi trường ảo:
```bash
python3 -m venv venv
source venv/bin/activate
```

### Bước 2: Cài đặt các thư viện phụ thuộc
```bash
pip install -r requirements.txt
```

### Bước 3: Chạy ứng dụng

*   **Cách 1: Chạy giao diện Web App Streamlit (Có Dashboard & Đồ thị trực quan)**:
    ```bash
    python3 -m streamlit run app_lead_scoring.py
    ```
    *Mở trình duyệt truy cập: `http://localhost:8501`. Hệ thống hiển thị biểu đồ phân loại, thống kê KPI và giao diện kiểm duyệt chỉnh sửa thủ công.*

*   **Cách 2: Chạy dòng lệnh offline (Không cần giao diện)**:
    ```bash
    python3 process_leads.py
    ```
    *Kết quả báo cáo Excel sẽ được ghi và xuất thẳng ra file `bao_cao_lead_scoring_bat_dong_san.xlsx`.*
