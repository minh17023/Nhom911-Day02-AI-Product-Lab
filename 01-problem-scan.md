# 01. Quét Cơ Hội & Đánh Giá Nhanh (Problem Scan & Quick-Assess)

## Phase 1: Bảng quét cơ hội vận hành tại các đơn vị Vingroup

| # | Subsidiary | Lens | Mô tả ngắn bài toán |
|---|---|---|---|
| 1 | Vinhomes | Lặp lại | Phân loại và điều hướng phản ánh sự cố kỹ thuật (điện, nước, điều hòa) từ cư dân trên ứng dụng Vinhomes Resident đến đúng tổ thợ kỹ thuật tòa nhà. |
| 2 | VinFast | AI có thể tốt hơn | Chẩn đoán sơ bộ tình trạng hỏng hóc từ câu mô tả tiếng Việt tự do của chủ xe điện VinFast để đề xuất đúng vật tư trước khi xe vào xưởng dịch vụ. |
| 3 | Vinmec | Tốn thời gian | Bác sĩ mất 25 đến 30 phút cho mỗi ca xuất viện để đọc lại toàn bộ xét nghiệm và viết bản tóm tắt hồ sơ bệnh án xuất viện. |
| 4 | Vinpearl | Đau từ người khác | Khách lưu trú gửi phản hồi chất lượng phòng trên các trang OTA; quản lý khách sạn mất 4 tiếng mỗi ngày để đọc thủ công và phân loại các khiếu nại khẩn cấp. |
| 5 | VinUni | Lặp lại | Giảng viên và trợ giảng mất nhiều giờ đọc code bài tập của sinh viên để chỉ ra lỗi logic cú pháp và viết phản hồi giải thích chi tiết. |

---

## Phase 2: Thẻ đánh giá nhanh (Quick Problem Cards)

### Thẻ bài toán 1: Vinhomes Phân loại và điều phối sự cố kỹ thuật căn hộ

┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #1                                       │
│                                                             │
│ Bài toán: Cư dân gửi phản ánh hỏng hóc thiết bị căn hộ      │
│ cần phân loại chính xác để điều phối thợ kỹ thuật tòa nhà.  │
│ Công ty thành viên: [x] Vinhomes                            │
│                                                             │
│ Ai đang đau (Actor)? Cư dân (chờ lâu), Lễ tân (quá tải)     │
│                                                             │
│ Workflow thủ công hiện tại (4 bước):                        │
│   1. Cư dân gửi tin nhắn mô tả lỗi kèm ảnh qua ứng dụng     │
│   2. Lễ tân đọc tin nhắn và suy đoán thuộc tổ điện hay nước │
│   3. Lễ tân viết phiếu công việc chuyển sang ban kỹ thuật   │
│   4. Tổ trưởng phân công thợ cầm dụng cụ lên căn hộ xử lý   │
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất? 2 và 3 (⏱ 35 phút/phiếu)  │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 2 và 3           │
│ (Trích xuất lỗi -> Phân loại tổ thợ -> Tạo phiếu nháp)      │
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                       │
│ Giảm thời gian điều phối từ 35 phút xuống dưới 2 phút.      │
│                                                             │
│ Quick Architecture: [x] LLM Feature                         │
└─────────────────────────────────────────────────────────────┘

### Thẻ bài toán 2: VinFast Đặt hẹn và chuẩn bị phụ tùng xưởng dịch vụ

┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #2                                       │
│                                                             │
│ Bài toán: Chủ xe VinFast mô tả hiện tượng lạ trên xe bằng   │
│ tiếng Việt đời thường, xưởng dịch vụ cần chuẩn bị phụ tùng. │
│ Công ty thành viên: [x] VinFast                             │
│                                                             │
│ Ai đang đau (Actor)? Chủ xe, Cố vấn dịch vụ (Service Advisor)│
│                                                             │
│ Workflow thủ công hiện tại (4 bước):                        │
│   1. Khách gọi hotline mô tả tiếng ồn hoặc lỗi hiển thị xe  │
│   2. Cố vấn dịch vụ tra cứu sổ tay kỹ thuật dòng xe (VF6/8/9)│
│   3. Cố vấn gọi điện xuống kho kiểm tra tồn kho phụ tùng    │
│   4. Xác nhận lịch hẹn tiếp nhận xe vào xưởng               │
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất? Bước 2 và 3 (⏱ 20 phút/khách)│
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 2 và 3           │
│ (Chuẩn hóa câu mô tả thành mã kỹ thuật và kiểm tra tồn kho) │
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                       │
│ Giảm tỷ lệ xe đến xưởng phải nằm chờ phụ tùng từ 28% về 5%. │
│                                                             │
│ Quick Architecture: [x] LLM Feature                         │
└─────────────────────────────────────────────────────────────┘

### Thẻ bài toán 3: Vinmec Soạn thảo tóm tắt hồ sơ xuất viện (Discharge Summary)

┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #3                                       │
│                                                             │
│ Bài toán: Trích xuất lịch sử điều trị và xét nghiệm để lập  │
│ bản tóm tắt xuất viện dễ hiểu cho bệnh nhân nội trú.        │
│ Công ty thành viên: [x] Vinmec                              │
│                                                             │
│ Ai đang đau (Actor)? Bác sĩ điều trị (quá tải hành chính)   │
│                                                             │
│ Workflow thủ công hiện tại (4 bước):                        │
│   1. Bác sĩ mở nhiều tab bệnh án điện tử, kết quả sinh hóa  │
│   2. Tổng hợp các mốc tiến triển bệnh lý vào tài liệu Word  │
│   3. Viết đơn thuốc xuất viện và lời dặn chế độ ăn uống     │
│   4. In ra giấy, kiểm tra lại toàn bộ và ký duyệt           │
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất? Bước 2 và 3 (⏱ 25 phút/ca)│
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 2 và 3           │
│ (Tổng hợp dữ liệu thô thành bản nháp chuẩn mẫu y khoa)      │
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                       │
│ Rút ngắn thời gian soạn thảo hồ sơ từ 25 phút xuống 5 phút. │
│                                                             │
│ Quick Architecture: [x] LLM Feature                         │
└─────────────────────────────────────────────────────────────┘