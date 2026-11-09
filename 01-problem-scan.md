# Tên nhóm: Nhóm 911
# Thành viên:
# - [Họ và tên] | [email@example.com]

---

# 01 — Problem Scan: Ý tưởng cá nhân (Vin Smart Future)

---

## 🔍 Phase 1 — SCAN: Bảng quét cơ hội

| # | Subsidiary | Lens | Mô tả ngắn bài toán |
|---|------------|------|---------------------|
| 1 | **Vinhomes** | Lặp lại | Nhân viên CSKH phân loại thủ công hàng trăm phản ánh của cư dân gửi qua App Vinhomes Resident mỗi ngày (mất nước, hỏng thang máy, ồn ào...) rồi forward đúng bộ phận xử lý — lặp lại, dễ nhầm. |
| 2 | **VinFast** | AI có thể tốt hơn | Khách hàng mô tả tiếng Việt tự nhiên về triệu chứng xe (ví dụ: *"xe đi qua gờ giảm tốc kêu cụp cụp ở bánh trước"*), kỹ thuật viên phải tự tra cứu mã lỗi — chậm và phụ thuộc kinh nghiệm cá nhân. |
| 3 | **Vinhomes** | Tốn thời gian | Nhân viên soạn thủ công phản hồi cho từng đánh giá 1-sao trên App, mỗi phản hồi mất 10–15 phút và chất lượng văn phong không đồng đều giữa các ca trực. |
| 4 | **Xanh SM** | Pain từ người khác | Tài xế phàn nàn hệ thống gợi ý điểm đón không chính xác vào giờ cao điểm, dẫn đến hủy chuyến và giảm thu nhập — dữ liệu GPS và feedback có nhưng chưa được khai thác. |
| 5 | **Vinmec** | Tốn thời gian | Bác sĩ mất 20–30 phút/bệnh nhân để soạn tóm tắt hồ sơ xuất viện từ bệnh án điện tử, trong khi thông tin đã có sẵn đầy đủ trong hệ thống HIS. |

---

## 🃏 Phase 2 — QUICK-ASSESS: 3 Quick Problem Cards

Chọn top 3 bài toán từ danh sách trên: **#1 (Vinhomes CSKH), #2 (VinFast Chẩn đoán lỗi), #3 (Vinhomes Phản hồi 1-sao)**.

```
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #1                                       │
│                                                             │
│ Bài toán: Tự động phân loại & điều hướng phản ánh cư dân   │
│ gửi qua App Vinhomes Resident đến đúng bộ phận xử lý.      │
│ Công ty thành viên: [ ] VinFast  [ ] Xanh SM  [x] Vinhomes │
│                                                             │
│ Ai đang đau (Actor)?                                        │
│   Nhân viên CSKH Vinhomes (phân loại thủ công, dễ nhầm)    │
│   và Cư dân (phản ánh không được xử lý đúng hạn).          │
│                                                             │
│ Workflow thủ công hiện tại (4 bước):                        │
│   1. Cư dân gửi phản ánh qua App (văn bản tự do)           │
│   ──> 2. CSKH đọc, phân loại danh mục thủ công             │
│   ──> 3. CSKH forward cho đúng bộ phận (Kỹ thuật/Vệ sinh…) │
│   ──> 4. Bộ phận tiếp nhận, phản hồi cư dân                │
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất? Bước 2–3 (⏱ 8 phút/lượt) │
│ AI có thể nhảy vào hỗ trợ ở bước nào?                      │
│   Bước 2: Đọc nội dung → phân loại danh mục + mức độ ưu    │
│   tiên → gắn tag → auto-route đến đúng bộ phận.            │
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                       │
│   Giảm thời gian phân loại từ 8 phút ──> dưới 30 giây.     │
│   Độ chính xác phân loại đúng danh mục đạt ≥ 92%.          │
│                                                             │
│ Quick Architecture: [ ] No AI  [ ] Rule  [x] LLM  [ ] Agent│
└─────────────────────────────────────────────────────────────┘
```

```
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #2                                       │
│                                                             │
│ Bài toán: Chẩn đoán sơ bộ mã lỗi kỹ thuật từ mô tả        │
│ tiếng Việt tự nhiên của khách hàng VinFast.                 │
│ Công ty thành viên: [x] VinFast  [ ] Xanh SM  [ ] Vinhomes │
│                                                             │
│ Ai đang đau (Actor)?                                        │
│   Kỹ thuật viên tiếp nhận (phải tra cứu tay), Khách hàng   │
│   (chờ lâu, phải giải thích lại nhiều lần).                 │
│                                                             │
│ Workflow thủ công hiện tại (4 bước):                        │
│   1. Khách gọi/chat mô tả triệu chứng bằng tiếng Việt      │
│   ──> 2. Kỹ thuật viên nghe, diễn giải, tra sổ mã lỗi      │
│   ──> 3. Đề xuất danh mục lỗi ban đầu và hành động tiếp    │
│   ──> 4. Chuyển lên kỹ sư cấp cao nếu không chắc chắn      │
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất? Bước 2 (⏱ 10 phút/lượt)  │
│ AI có thể nhảy vào hỗ trợ ở bước nào?                      │
│   Bước 2: NLP → map mô tả → gợi ý top-3 mã lỗi kèm        │
│   hành động khuyến nghị để kỹ thuật viên xác nhận.         │
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                       │
│   Giảm thời gian tra cứu từ 10 phút ──> dưới 2 phút.       │
│   Top-3 mã lỗi gợi ý chứa đúng mã thực tế ≥ 85% trường    │
│   hợp (kỹ thuật viên xác nhận cuối cùng).                  │
│                                                             │
│ Quick Architecture: [ ] No AI  [ ] Rule  [x] LLM  [ ] Agent│
└─────────────────────────────────────────────────────────────┘
```

```
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #3                                       │
│                                                             │
│ Bài toán: Tự động soạn thảo nháp phản hồi cư dân cho các   │
│ đánh giá 1-sao trên App Vinhomes Resident.                  │
│ Công ty thành viên: [ ] VinFast  [ ] Xanh SM  [x] Vinhomes │
│                                                             │
│ Ai đang đau (Actor)?                                        │
│   Nhân viên CSKH (mất nhiều thời gian soạn, không đồng     │
│   đều chất lượng) và Cư dân (cảm thấy không được lắng nghe)│
│                                                             │
│ Workflow thủ công hiện tại (3 bước):                        │
│   1. CSKH đọc từng đánh giá 1-sao                          │
│   ──> 2. Soạn thủ công phản hồi cá nhân hóa                │
│   ──> 3. Supervisor duyệt, chỉnh sửa và gửi đi             │
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất? Bước 2 (⏱ 12 phút/lượt)  │
│ AI có thể nhảy vào hỗ trợ ở bước nào?                      │
│   Bước 2: Đọc nội dung đánh giá → draft phản hồi đồng cảm, │
│   chuyên nghiệp, cá nhân hóa theo vấn đề cụ thể.           │
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                       │
│   Giảm thời gian soạn thảo từ 12 phút ──> dưới 2 phút.     │
│   Tỉ lệ bản nháp được duyệt mà không cần sửa đáng kể ≥ 80%.│
│                                                             │
│ Quick Architecture: [ ] No AI  [ ] Rule  [x] LLM  [ ] Agent│
└─────────────────────────────────────────────────────────────┘
```

