# BÁO CÁO KỸ THUẬT GIAI ĐOẠN PHÁT TRIỂN — [TÊN GIAI ĐOẠN / PHASE XX]

---

## SECTION 1 — PROJECT INFORMATION

* **Tên dự án (Project Name):** SystemBoost Pro
* **Giai đoạn hiện tại (Current Phase):** Phase XX — [Tên giai đoạn]
* **Ngày báo cáo (Report Date):** YYYY-MM-DD
* **Nhánh Git (Git Branch):** `<branch-name>`
* **Commit SHA mới nhất (Latest Commit SHA):** `<commit-sha>`
* **Môi trường phát triển (Development Environment):** Windows PowerShell / Visual Studio / Antigravity IDE
* **Phiên bản .NET SDK (.NET SDK Version):** `<kết quả thực tế từ dotnet --info>`
* **Hệ điều hành (Operating System):** `<kết quả thực tế từ Environment.OSVersion>`

---

## SECTION 2 — PHASE OBJECTIVES

### 2.1. Yêu cầu gốc (Original Requirements)
* [Liệt kê đầy đủ các yêu cầu đầu vào của giai đoạn]

### 2.2. Sản phẩm bàn giao dự kiến (Expected Deliverables)
* [Liệt kê các tệp, mô-đun, tài liệu hoặc tính năng cần bàn giao]

### 2.3. Tiêu chí nghiệm thu (Acceptance Criteria)
* [Liệt kê các điều kiện cụ thể để giai đoạn được đánh giá hoàn thành]

### 2.4. Phạm vi phát triển (Development Scope)
* **Trong phạm vi (In-Scope):** [...]
* **Ngoài phạm vi (Out-of-Scope):** [...]

---

## SECTION 3 — COMPLETED FEATURES

> **Quy tắc trạng thái:** Chỉ sử dụng `COMPLETED`, `PARTIAL`, `NOT IMPLEMENTED`, hoặc `BLOCKED`. Tuyệt đối không đánh dấu `COMPLETED` nếu không có bằng chứng thực thi cụ thể.

| Tên tính năng (Feature Name) | Mô tả (Description) | Trạng thái (Implementation Status) | Tệp mã nguồn chính (Main Source Files) | Trạng thái kiểm thử (Testing Status) | Bằng chứng xác minh (Verification Evidence) |
| :--- | :--- | :--- | :--- | :--- | :--- |
| `[Feature 1]` | `[Mô tả chi tiết]` | `COMPLETED / PARTIAL / NOT IMPLEMENTED / BLOCKED` | `[Đường dẫn tệp]` | `[Trạng thái test]` | `[reports/evidence/...]` |

---

## SECTION 4 — SOURCE CODE CHANGES

### 4.1. Danh sách tệp thay đổi
* **Tệp mới tạo (New Files):**
  * `[đường dẫn tệp]` — `[Mục đích]`
* **Tệp chỉnh sửa (Modified Files):**
  * `[đường dẫn tệp]` — `[Mục đích chỉnh sửa]`
* **Tệp đã xóa (Removed Files):**
  * `[Không có / đường dẫn tệp]`

### 4.2. Các lớp và dịch vụ quan trọng (Important Classes & Services)
* `[Tên Class / Interface / Service]` (`[đường dẫn tệp]`): `[Giải thích vai trò và trách nhiệm]`

### 4.3. Thay đổi kiến trúc (Architectural Changes)
* `[Mô tả chi tiết các thay đổi về kiến trúc hoặc luồng điều khiển]`

---

## SECTION 5 — BUILD VERIFICATION

> **Quy tắc bắt buộc:** Nếu không thực thi lệnh build trong giai đoạn này, phải ghi rõ **`BUILD NOT EXECUTED`**. Không bao giờ báo cáo build thành công nếu không có log thực tế.

* **Trạng thái thực thi Build:** `[BUILD EXECUTED / BUILD NOT EXECUTED]`
* **Lệnh Build chính xác (Exact Build Command):** `<dotnet build ... hoặc N/A>`
* **Môi trường Build (Build Environment):** `<OS, .NET SDK>`
* **Kết quả Build (Build Result):** `<Succeeded / Failed / BUILD NOT EXECUTED>`
* **Số lượng lỗi (Number of Errors):** `<0 / N/A>`
* **Số lượng cảnh báo (Number of Warnings):** `<0 / N/A>`
* **Đường dẫn Log Build đầy đủ (Complete Build Log Path):** `reports/evidence/<build-log-file>.log`

### Trích đoạn Log Build (Build Log Excerpts)
```text
[Dán trích đoạn thực tế từ tệp log trong reports/evidence/ hoặc ghi rõ BUILD NOT EXECUTED]
```

---

## SECTION 6 — TEST RESULTS

> **Lưu ý:** Không đánh đồng việc biên dịch thành công (compilation) với kiểm thử thời gian chạy (runtime testing). Nếu không chạy test, phải giải thích rõ lý do.

* **Danh mục kiểm thử (Test Categories):** `[Unit Tests / Integration Tests / Infrastructure Verification]`
* **Lệnh kiểm thử (Test Commands):** `<dotnet test ... hoặc lệnh xác minh>`
* **Tổng số bài kiểm thử đã chạy (Total Tests Executed):** `<số lượng>`
* **Số bài đạt (Passed Tests):** `<số lượng>`
* **Số bài thất bại (Failed Tests):** `<số lượng>`
* **Số bài bỏ qua (Skipped Tests):** `<số lượng>`
* **Kết quả kiểm thử tích hợp (Integration Test Results):** `[Chi tiết]`
* **Kết quả kiểm tra thủ công (Manual Verification Results):** `[Chi tiết kèm đường dẫn tới reports/evidence/]`

---

## SECTION 7 — UI VERIFICATION

> **Quy tắc bắt buộc:** Không sử dụng ảnh thiết kế giả lập (mockup) làm bằng chứng ứng dụng đang chạy. Lưu ảnh chụp màn hình thực tế tại `reports/evidence/`.

* **Trạng thái kiểm tra UI:** `[APPLICABLE / NOT APPLICABLE IN THIS PHASE]`
* **Ảnh chụp màn hình (Screenshots):** `reports/evidence/<screenshot>.png`
* **Các màn hình đã xác minh (Verified Screens):** `[Danh sách màn hình]`
* **Lỗi hiển thị phát hiện được (Visual Defects):** `[Danh sách lỗi hoặc Không có]`
* **Thành phần UI chưa hoàn thiện (Unfinished UI Components):** `[Danh sách]`
* **Kiểm tra điều hướng (Navigation Behavior):** `[Kết quả]`
* **Kiểm tra chuyển đổi giao diện (Theme Switching):** `[Kết quả]`

---

## SECTION 8 — KNOWN ISSUES

> **Mức độ nghiêm trọng (Severity Levels):** `CRITICAL`, `HIGH`, `MEDIUM`, `LOW`.

| Mã lỗi | Mô tả vấn đề (Issue Description) | Mức độ (Severity) | Thành phần ảnh hưởng (Affected Component) | Các bước tái hiện (Reproduction Steps) | Giải pháp đề xuất (Proposed Solution) | Trạng thái (Current Status) |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| `ISS-01` | `[Mô tả]` | `CRITICAL / HIGH / MEDIUM / LOW` | `[Thành phần]` | `[Các bước tái hiện]` | `[Giải pháp]` | `OPEN / RESOLVED` |

---

## SECTION 9 — SECURITY AND WINDOWS SAFETY

* **Yêu cầu quyền Quản trị viên (Administrator Privilege Requirements):** `[Đánh giá]`
* **Hành vi xóa tệp (File Deletion Behavior):** `[Đánh giá]`
* **Thay đổi Windows Registry (Registry Modifications):** `[Đánh giá]`
* **Thay đổi dịch vụ Windows (Windows Service Modifications):** `[Đánh giá]`
* **Thay đổi cấu hình hệ thống (System Configuration Changes):** `[Đánh giá]`
* **Sao lưu và phục hồi dữ liệu (Data Backup and Recovery):** `[Đánh giá]`
* **Yêu cầu xác nhận từ người dùng (User Confirmation Requirements):** `[Đánh giá]`
* **Xử lý lỗi (Error Handling):** `[Đánh giá]`
* **Rủi ro mất dữ liệu tiềmẩn (Potential Data Loss Risks):** `[Đánh giá]`

---

## SECTION 10 — PERFORMANCE REVIEW

> **Quy tắc bắt buộc:** Tuyệt đối không tự bịa số liệu benchmark. Nếu không đo đạc hiệu năng trong giai đoạn này, phải ghi rõ là **chưa đo đạc hiệu năng**.

* **Thời gian khởi động ứng dụng (Application Startup Time):** `[Số liệu thực tế hoặc Chưa đo đạc]`
* **Mức tiêu thụ bộ nhớ (Memory Consumption):** `[Số liệu thực tế hoặc Chưa đo đạc]`
* **Mức sử dụng CPU (CPU Usage):** `[Số liệu thực tế hoặc Chưa đo đạc]`
* **Độ phản hồi giao diện (UI Responsiveness):** `[Số liệu thực tế hoặc Chưa đo đạc]`
* **Hiệu năng tác vụ nền (Background Operation Performance):** `[Số liệu thực tế hoặc Chưa đo đạc]`
* **Phương pháp thu thập số liệu (Measurement Methodology):** `[Mô tả cách đo hoặc ghi rõ Performance was not measured]`

---

## SECTION 11 — UNFINISHED WORK

* **Tính năng chưa hoàn thiện (Incomplete Features):** `[Danh sách]`
* **Yêu cầu còn thiếu (Missing Requirements):** `[Danh sách]`
* **Giới hạn kỹ thuật (Technical Limitations):** `[Danh sách]`
* **Cải tiến tạm hoãn (Deferred Improvements):** `[Danh sách]`
* **Lỗi tồn đọng (Outstanding Defects):** `[Danh sách]`

---

## SECTION 12 — SELF-ASSESSMENT

### 12.1. Sự thật đã xác minh (Verified Facts)
* `[Liệt kê các sự thật kỹ thuật có bằng chứng trực tiếp trong kho mã và reports/evidence/]`

### 12.2. Đánh giá kỹ thuật chủ quan (Subjective Technical Assessment)
* **Chất lượng mã nguồn (Code Quality):** `[Đánh giá]`
* **Kiến trúc (Architecture):** `[Đánh giá]`
* **Độ hoàn thiện chức năng (Functional Completeness):** `[Đánh giá]`
* **Độ tin cậy (Reliability):** `[Đánh giá]`
* **Bảo mật & An toàn (Security):** `[Đánh giá]`
* **Khả năng bảo trì (Maintainability):** `[Đánh giá]`
* **Độ bao phủ kiểm thử (Test Coverage):** `[Đánh giá]`

---

## SECTION 13 — NEXT PHASE RECOMMENDATIONS

* **Các tác vụ khuyến nghị tiếp theo (Recommended Next Tasks):** `[Danh sách]`
* **Các phụ thuộc kỹ thuật (Dependencies):** `[Danh sách]`
* **Rủi ro kỹ thuật (Technical Risks):** `[Danh sách]`
* **Thứ tự triển khai đề xuất (Suggested Implementation Order):** `[Danh sách theo thứ tự ưu tiên]`

---

## SECTION 14 — SUPERVISOR REVIEW REQUEST

**SUPERVISOR REVIEW REQUIRED**

* **Các hạng mục cần Supervisor đánh giá (Features Requiring Review):**
  * `[Danh sách]`
* **Các vấn đề chưa giải quyết (Known Unresolved Issues):**
  * `[Danh sách]`
* **Các quyết định kỹ thuật quan trọng (Important Technical Decisions):**
  * `[Danh sách]`
* **Bằng chứng Build và Kiểm thử (Build and Testing Evidence):**
  * `[Danh sách đường dẫn trong reports/evidence/]`
* **Câu hỏi cần Supervisor cho ý kiến (Questions Requiring Supervisor Assessment):**
  * `[Danh sách câu hỏi kỹ thuật]`
