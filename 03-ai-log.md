# Tên nhóm: Nhóm 911
# Thành viên:
# - [Mai Lê Bá Vương] | [mlbv2005@gmail.com]

---

# 03 — AI Log: Nhật ký tương tác AI trong Lab 02

## 1. Đã dùng AI để làm gì trong lab này?

### 🔍 Đọc hiểu yêu cầu bài lab
Đầu tiên nhờ AI đọc toàn bộ `README.md` và cấu trúc thư mục, sau đó giải thích rõ từng thành viên cần làm gì, tại sao trưởng nhóm phải merge, và quy tắc đặt tên/header file là ở đâu. Phần này AI làm tốt vì chỉ cần đọc và tổng hợp — không cần sáng tạo.

### 📄 Đọc slide hướng dẫn để bổ sung ngữ cảnh
Slide của giảng viên có thông tin quan trọng mà README không đề cập, cụ thể là: *"Tại đầu mỗi file, bắt buộc phải điền đầy đủ và chính xác: Tên nhóm, Họ và tên kèm email của từng thành viên."* AI đã phát hiện sự khác biệt giữa README và slide, và chỉ ra nên ưu tiên theo slide.

### 💻 Hoàn thiện file code `prompt_prototype.py`
AI điền 2 phần còn TODO trong file code:
- **`SYSTEM_PROMPT`:** Viết system instruction cho vai trò dispatcher co-pilot Xanh SM, với 2 safety rules (DRAFT_ONLY tag và critical battery < 5%).
- **Hàm `evaluate_prompt()`:** Implement gọi Gemini API, có Option A (SDK mới `google-genai`) và Option B (SDK cũ `google-generativeai`) làm fallback.

Nội dung code lấy từ slide giảng viên — AI chỉ đọc và điền vào đúng vị trí, không tự sáng tác.
---

## 2. AI đã sai / cần sửa ở đâu?

### ❌ Lần 1: Điền nhầm vào worksheet thay vì tạo file riêng
AI đã điền nội dung bài làm thẳng vào `01-worksheet.md` (file template gốc của giảng viên), trong khi yêu cầu đúng là phải **tạo file mới** (`01-problem-scan.md`, `02-deep-dive-report.md`). Người dùng phải nhắc lại: *"Lấy README.md làm chuẩn. Tạo các file tương ứng."*

**Bài học:** AI cần đọc kỹ README để phân biệt file template (không được sửa) và file nộp bài (cần tạo mới). Việc này đã khiến phải restore lại `01-worksheet.md` về trạng thái gốc bằng lệnh git.

### ❌ Lần 2: File 03-ai-log.md viết không đúng thực tế
AI-log lần đầu viết theo kiểu *"giả sử tôi dùng Gemini để brainstorm"* — hư cấu, không phản ánh những gì thực sự xảy ra trong phiên làm việc. Người dùng phải yêu cầu viết lại: *"Cái AI-log tức là những gì trong bài lab này đã dùng AI làm gì, đã sửa gì, học được gì."*

**Bài học:** AI-log phải trung thực, ghi lại đúng những tương tác thực tế — không được viết như một bài mẫu chung chung.

---

## 3. Học được gì từ việc dùng AI trong lab này?

**AI giỏi nhất ở:** Đọc tài liệu dài và tổng hợp rõ ràng, điền code có sẵn vào đúng chỗ, viết theo format và ví dụ cho sẵn, tạo nội dung có cấu trúc (bảng, diagram text, Problem Statement).

**AI yếu nhất ở:** Phân biệt *"file nào cần sửa, file nào không"* khi chưa được chỉ rõ. Nếu không có người dùng kiểm tra, AI có thể sửa nhầm file template gốc — gây mất nội dung.

**Quy trình hiệu quả nhất:**
1. Cho AI đọc tài liệu → tổng hợp yêu cầu
2. Người dùng ra quyết định (chọn chủ đề, chốt hướng đi)
3. AI thực thi theo hướng đã chốt
4. Người dùng review và điều chỉnh nếu lệch

---

## 4. Kết luận

AI đóng vai trò **công cụ tăng tốc** trong lab này — phần đọc hiểu, soạn thảo, và điền form thực hiện nhanh hơn nhiều lần so với làm tay. Tuy nhiên, vai trò ra quyết định (chọn đề tài #5 và #6, chốt bài toán Deep-Dive là Vinhomes) vẫn hoàn toàn thuộc về người học. AI không tự chọn được bài toán phù hợp nếu không có định hướng từ người dùng.

Điều quan trọng nhất rút ra: **Prompt rõ = Output tốt.** Mỗi lần AI làm sai đều xuất phát từ việc yêu cầu chưa đủ cụ thể. Khi người dùng nói rõ *"lấy README làm chuẩn, tạo file mới"* thay vì chỉ nói *"làm bài"*, kết quả ngay lập tức đúng hướng.
