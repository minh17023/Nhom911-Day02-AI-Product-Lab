# 01 — Problem Scan & Quick Problem Cards

**Nhóm:** Nhom911
**Người thực hiện:** Nguyễn Hữu Thịnh — MSSV 2A202603726 — 26ai.thinhnh2@vinuni.edu.vn

**Thành viên nhóm:**

| Họ và tên | Mã sinh viên | Email | Vai trò |
| --- | --- | --- | --- |
| Nguyễn Hữu Thịnh | 2A202603726 | 26ai.thinhnh2@vinuni.edu.vn | Thành viên |

> Mọi con số baseline/target trong file là **giả định để minh họa**, cần thay bằng số liệu thực khi có.

---

## Phase 1 — SCAN: Tìm kiếm cơ hội

Dùng 4 lens (Lặp lại · Tốn thời gian · AI có thể tốt hơn · Pain từ người khác) quét vận hành của 5 công ty thành viên.

| #   | Subsidiary   | Lens               | Mô tả ngắn bài toán                                                                                                                                                       |
| --- | ------------ | ------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1   | **Vinhomes** | Lặp lại            | Lễ tân BQL đọc, phân loại và chuyển thủ công phản ánh cư dân từ 5 kênh (app, Zalo OA, hotline, quầy, email). ~11 phút thao tác/phản ánh, ~30% chuyển sai bộ phận lần đầu. |
| 2   | **Vinmec**   | Pain từ người khác | Bệnh nhân đặt lịch online phải tự chọn 1 trong 20–30 chuyên khoa; ~15% lượt khám phải chuyển khoa, và form không cảnh báo người có triệu chứng cấp cứu.                   |
| 3   | **Xanh SM**  | Tốn thời gian      | Điều phối viên xử lý thủ công sự cố hết pin giữa đường: tra vị trí, tìm trạm sạc trống, soạn chỉ dẫn. ~15 phút/lượt, ~80 sự cố/ngày tại Hà Nội.                           |
| 4   | **VinFast**  | AI có thể tốt hơn  | Khách mô tả lỗi xe bằng tiếng Việt tự do ("đi qua gờ giảm tốc kêu cụp cụp bánh trước"); CSKH phải hỏi lại nhiều lượt mới gán được mã lỗi ban đầu, ~10 phút/ca.            |
| 5   | **Vinpearl** | Pain từ người khác | Review tiêu cực khẩn ("phòng bẩn", "nhân viên thái độ") trên Booking/Agoda/Google Maps được phát hiện chậm 1–2 ngày, Manager không kịp xử lý khi khách còn lưu trú.       |
| 6   | **Vinmec**   | Tốn thời gian      | Bác sĩ mất 20–30 phút/bệnh nhân viết tóm tắt xuất viện từ bệnh án điện tử.                                                                                                |

---

## Phase 2 — QUICK-ASSESS: 3 Quick Problem Cards

Chọn top 3: **#1 Vinhomes**, **#2 Vinmec**, **#3 Xanh SM** — ba bài toán có workflow rõ, có điểm nghẽn đo được bằng phút, và mức rủi ro khác nhau để so sánh.

### Card #1 — Vinhomes: Tiếp nhận & định tuyến phản ánh cư dân

```text
┌──────────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #1                                            │
│                                                                  │
│ Bài toán: Phản ánh cư dân từ 5 kênh được phân loại và chuyển     │
│ bộ phận thủ công, chậm và hay chuyển nhầm.                       │
│ Công ty thành viên: [x] Vinhomes                                 │
│                                                                  │
│ Ai đang đau? Lễ tân / điều phối BQL (quá tải), cư dân (chờ lâu), │
│ các bộ phận kỹ thuật/vệ sinh/an ninh (nhận ticket sai).          │
│                                                                  │
│ Workflow thủ công hiện tại:                                      │
│   1. Cư dân gửi phản ánh qua 1 trong 5 kênh                      │
│   → 2. Lễ tân đọc, xác định tòa/căn, loại sự cố (⏱ 4')           │
│   → 3. Nhập ticket vào hệ thống (⏱ 3')                           │
│   → 4. Chọn bộ phận, chuyển ticket (⏱ 2', ~30% sai)              │
│   → 5. Phản hồi cư dân khi xử lý xong (⏱ 2')                     │
│                                                                  │
│ Bước tốn nhất: Bước 2–4 (⏱ 9 phút/lượt, +30' khi chuyển sai)     │
│ AI nhảy vào: Bước 2–4 (trích xuất tòa/căn/loại, gợi ý bộ phận)   │
│ và soạn nháp tin xác nhận cho cư dân.                            │
│                                                                  │
│ Metric có số:                                                    │
│ - Định tuyến đúng lần đầu: 70% ──> ≥ 90%                         │
│ - Thời gian cư dân nhận xác nhận: 2–4 giờ ──> ≤ 15 phút          │
│ - Thao tác lễ tân: 11 phút ──> ≤ 3 phút (chỉ duyệt)              │
│                                                                  │
│ Quick Architecture: [ ] No AI  [ ] Rule  [x] LLM  [ ] Agent      │
│ (LLM trích xuất + soạn nháp; rule cho khẩn cấp, gộp trùng, SLA)  │
└──────────────────────────────────────────────────────────────────┘
```

### Card #2 — Vinmec: Trợ lý phân luồng triệu chứng trước khi đặt lịch

```text
┌──────────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #2                                            │
│                                                                  │
│ Bài toán: Bệnh nhân tự chọn chuyên khoa khi đặt lịch online, dễ  │
│ chọn sai và không có cơ chế phát hiện ca cấp cứu.                │
│ Công ty thành viên: [x] Vinmec                                   │
│                                                                  │
│ Ai đang đau? Bệnh nhân (chọn sai khoa, chờ lại), điều dưỡng      │
│ tổng đài (trả lời lặp "khám khoa nào"), bác sĩ (slot bị lãng phí)│
│                                                                  │
│ Workflow thủ công hiện tại:                                      │
│   1. Mở app, vào màn đặt lịch (⏱ 1')                             │
│   → 2. Tự chọn chuyên khoa trong 20–30 khoa (⏱ 3') 🔴            │
│   → 3. Chọn bác sĩ, giờ, xác nhận (⏱ 2')                         │
│   → 4. Đến khám; sai khoa → chuyển khoa, đặt lại (+1–3 ngày)     │
│   (Phân vân → gọi tổng đài, chờ điều dưỡng ⏱ 5–10')              │
│                                                                  │
│ Bước tốn/lỗi nhất: Bước 2 (⏱ 3 phút, ~25% chọn sai)              │
│ AI nhảy vào: Bước 2 — gợi ý 1–2 khoa; rule red flag chặn ca      │
│ cấp cứu TRƯỚC khi gọi LLM.                                       │
│                                                                  │
│ Metric có số:                                                    │
│ - Recall red flag ≥ 99% (điều kiện bắt buộc để go-live)          │
│ - Chuyển khoa sau khám: 15% ──> ≤ 7%                             │
│ - Thời gian đặt lịch: 6 phút ──> ≤ 4 phút                        │
│                                                                  │
│ Quick Architecture: [ ] No AI  [x] Rule  [x] LLM  [ ] Agent      │
│ (Rule giữ cửa an toàn, LLM chỉ gợi ý khoa)                       │
└──────────────────────────────────────────────────────────────────┘
```

### Card #3 — Xanh SM: Xử lý sự cố hết pin thực địa

```text
┌──────────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #3                                            │
│                                                                  │
│ Bài toán: Tài xế báo hết pin giữa đường, điều phối viên phải     │
│ tra cứu và soạn chỉ dẫn thủ công.                                │
│ Công ty thành viên: [x] Xanh SM                                  │
│                                                                  │
│ Ai đang đau? Tài xế (chờ, mất cuốc), điều phối viên (quá tải     │
│ giờ cao điểm).                                                   │
│                                                                  │
│ Workflow thủ công hiện tại:                                      │
│   1. Nhận cuộc gọi sự cố (⏱ 2')                                  │
│   → 2. Tra GPS xe (⏱ 2')                                         │
│   → 3. Tra trạm sạc VinFast còn trụ trống (⏱ 5') 🔴              │
│   → 4. Soạn tin chỉ dẫn gửi tài xế (⏱ 5') 🔴                     │
│   → 5. Gọi cứu hộ nếu pin quá thấp (⏱ 1')                        │
│                                                                  │
│ Bước tốn nhất: Bước 3–4 (⏱ 10 phút/lượt)                         │
│ AI nhảy vào: Bước 3–4 — tự lấy dữ liệu trạm, soạn nháp chỉ dẫn   │
│                                                                  │
│ Metric có số: Thời gian xử lý 15 phút ──> dưới 3 phút;           │
│ 98% chỉ dẫn đúng trạm và đúng loại cổng sạc.                     │
│                                                                  │
│ Quick Architecture: [ ] No AI  [ ] Rule  [x] LLM  [ ] Agent      │
└──────────────────────────────────────────────────────────────────┘
```

---

## Stress-test các thẻ (phản biện kiểu CFO / Trưởng phòng Vận hành)

Phản biện theo tinh thần prompt "đóng vai CFO và Trưởng phòng Vận hành khắt khe" trong worksheet, thực hiện cùng trợ lý AI (Claude) khi soạn thẻ. Các điểm yếu được chỉ ra và cách xử lý:

| Thẻ         | Phản biện                                                          | Cách xử lý                                                                                                                                                                                                   |
| ----------- | ------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| #1 Vinhomes | "Phân loại 6 bộ phận bằng từ khóa là đủ, cần gì LLM?"              | Từ khóa không xử lý được cách viết tòa/căn đa dạng ("S2-1205", "căn 12A05 tòa S2") và đồng nghĩa ("nhỏ giọt trên trần" = "thấm trần"). Giữ rule cho phần cần chính xác tuyệt đối (khẩn cấp, SLA, gộp trùng). |
| #1 Vinhomes | "30% chuyển sai lấy ở đâu?"                                        | Chưa có số thật → ghi rõ là giả định; bước đầu của lộ trình là gán nhãn 500 phản ánh cũ để đo baseline.                                                                                                      |
| #2 Vinmec   | "Một ca cấp cứu bị bỏ sót là thảm họa pháp lý."                    | Đồng ý — đây là lý do loại khỏi Deep-Dive ở giai đoạn này (xem dưới).                                                                                                                                        |
| #3 Xanh SM  | "Tìm trạm gần nhất còn trống là bài toán truy vấn, không cần LLM." | Đúng một phần: truy vấn trạm nên là code; LLM chỉ soạn tin chỉ dẫn.                                                                                                                                          |

---

## 🗳️ Bài toán được chọn để Deep-Dive

**Chọn Card #1 — Vinhomes: Tiếp nhận & định tuyến phản ánh cư dân.**

**Lý do chọn:**

- Điểm nghẽn rõ và đo được (9 phút thao tác + 30% chuyển sai), lặp lại hàng trăm lần mỗi ngày.
- Đúng thế mạnh của LLM: biến văn bản tự do thành bản ghi có cấu trúc.
- Rủi ro kiểm soát được bằng Human-in-the-loop (điều phối viên duyệt mọi tin gửi cư dân) và rule bọc ngoài.
- Có thể prototype và test tự động ngay (tag `[DRAFT_ONLY]`, regex số điện thoại, từ cấm) — đã chạy 8/8 test đạt (xem `02-deep-dive-report.md`).

**Lý do loại:**

- **Card #2 Vinmec:** hậu quả khi sai là tính mạng. Muốn làm đúng cần bộ 200 ca red flag do bác sĩ gán nhãn và hội đồng bác sĩ sở hữu danh sách red flag — hiện chưa có. Đánh giá trung thực ở thời điểm này là **NOT YET**, nên không phù hợp để Deep-Dive trong buổi lab.
- **Card #3 Xanh SM:** đã là worked example của giảng viên; phần lớn giá trị nằm ở truy vấn dữ liệu trạm sạc (code) hơn là LLM. Vẫn được dùng làm bài prototype chuẩn trong `starter-code/prompt_prototype.py`.
