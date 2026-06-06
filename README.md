# AI Lead Scoring & Automation - Hệ thống phân loại & Chấm điểm Khách hàng tiềm năng

Dự án này là hệ thống chấm điểm và phân loại khách hàng tiềm năng (Lead Scoring) trong ngành Bất động sản. Hệ thống tự động tải dữ liệu từ Google Sheets, phân tích nhu cầu khách hàng theo các tiêu chí nghiệp vụ quy định, sau đó xuất ra file báo cáo Excel được định dạng chuyên nghiệp để bàn giao.

Dự án này chạy hoàn toàn ngoại tuyến (offline) bằng Python mà không cần kết nối API hoặc giao diện phức tạp.

---

## 📁 Cấu Trúc Thư Mục Dự Án

*   `process_leads.py`: Script Python chính thực hiện tải dữ liệu, chạy thuật toán chấm điểm và xuất file Excel.
*   `tieu_chi_cham_diem.txt`: Tài liệu mô tả các tiêu chí nghiệp vụ cộng/trừ điểm và phân loại thô.
*   `lead_scoring_skill.md`: Tài liệu hướng dẫn định dạng Skill AI chi tiết để phát triển hệ thống.
*   `requirements.txt`: Danh sách các thư viện Python phụ thuộc tối giản cần thiết.
*   `.gitignore`: File cấu hình Git để bỏ qua các thư mục bộ nhớ đệm và file kết xuất trung gian.

---

## 📋 Tiêu Chí Chấm Điểm (Business Rules)

Hệ thống bắt đầu từ mức điểm mặc định là **0 điểm** và áp dụng quy tắc sau:

1.  **Cộng 50 Điểm (Khách hàng VIP / Siêu Tiềm Năng)**:
    *   **Ngân sách**: Từ 20 tỷ trở lên hoặc "tài chính mạnh", "ngân sách không giới hạn".
    *   **Loại hình cao cấp**: Biệt thự đơn lập, Penthouse, Shophouse mặt tiền lớn, Quỹ đất công nghiệp, Sàn văn phòng diện tích lớn.
    *   **Vị trí**: Quận 1, Ven sông, Vinhomes Ocean Park, Phú Mỹ Hưng, khu Đông.
    *   **Đối tượng**: Chủ doanh nghiệp, Nhà đầu tư chuyên nghiệp, Mua sỉ, gom 5-10 căn.
    *   **Tính cấp thiết**: Pháp lý chuẩn 100%, Sổ hồng riêng, muốn gặp trực tiếp chủ đầu tư để đàm phán.
2.  **Trừ 50 Điểm (Khách hàng Rác / Không Tiềm Năng)**:
    *   **Yêu cầu phi thực tế**: Tìm mua nhà Quận 1 giá 1-2 tỷ, nhà trung tâm có sân vườn giá vài trăm triệu, thuê nhà trung tâm giá 2 triệu.
    *   **Không nhu cầu**: Nhầm số, nhầm ngành, dữ liệu cũ.
    *   **Thiếu thiện chí**: Hỏi giá cho vui, chưa muốn mua, thái độ không hợp tác.
    *   **Spam / Mời chào**: Bảo hiểm, vay vốn, mời chào dịch vụ khác.
    *   **Lỗi liên lạc**: Thuê bao, gọi không bắt máy, không trả lời Zalo.
3.  **Giữ Nguyên 0 Điểm (Khách hàng Bình thường)**:
    *   Mua chung cư, nhà phố tầm trung (3-10 tỷ).
    *   Khách cần vay ngân hàng, đang cân nhắc chính sách.
    *   Thuê mặt bằng spa Quận 1 dưới 50 triệu/tháng.
    *   Khách mua đất nền vùng ven (Long An, Đồng Nai) tài chính 2-3 tỷ.

---

## 🚀 Hướng Dẫn Cài Đặt & Chạy Offline

### Bước 1: Chuẩn bị môi trường Python

Mở Terminal và điều hướng đến thư mục dự án:
```bash
cd "/Users/lyhoangphuc/Documents/[MindX School] PNL- AI4A01 - Google Drive/07. Lesson 7/Demo07"
```

Khởi tạo và kích hoạt môi trường ảo (Khuyên dùng):
```bash
python3 -m venv venv
source venv/bin/activate
```

### Bước 2: Cài đặt các thư viện phụ thuộc
Cài đặt nhanh từ file `requirements.txt`:
```bash
pip install -r requirements.txt
```

### Bước 3: Chạy script chấm điểm
Thực thi lệnh Python để tự động tải dữ liệu từ Google Sheets và xuất file Excel:
```bash
python3 process_leads.py
```

Sau khi chạy xong, file báo cáo hoàn chỉnh được định dạng chi tiết sẽ được xuất ra tại: **`bao_cao_lead_scoring_bat_dong_san.xlsx`** trong cùng thư mục.

---

## 🐙 Hướng Dẫn Đẩy Lên GitHub

Để đẩy mã nguồn này lên kho lưu trữ GitHub của bạn, hãy thực hiện các lệnh sau:

1.  **Khởi tạo Git** (nếu chưa khởi tạo):
    ```bash
    git init
    ```

2.  **Liên kết với kho chứa trên GitHub** (Thay thế bằng đường link repository của bạn):
    ```bash
    git remote add origin https://github.com/username/ten-kho-chua.git
    ```

3.  **Kiểm tra các file đang chuẩn bị commit**:
    ```bash
    git status
    ```
    *(Các file kết xuất báo cáo và môi trường ảo `venv` sẽ tự động bị bỏ qua nhờ cấu hình trong `.gitignore`).*

4.  **Thêm các file và commit**:
    ```bash
    git add .
    git commit -m "feat: setup lead scoring offline python script and documentation"
    ```

5.  **Đẩy lên nhánh chính (main)**:
    ```bash
    git branch -M main
    git push -u origin main
    ```
