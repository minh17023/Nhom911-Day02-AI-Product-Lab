# Tên nhóm: Nhóm 911
# Thành viên:
# - [Mai Lê Bá Vương] | [mlbv2005@gmail.com]

---

# 02 — Deep Dive Report: Phân tích sâu dự án AI (Vin Smart Future)

> **Bài toán được chọn:** Phân loại & Điều hướng phản ánh cư dân — **Vinhomes**
> **Mảng kinh doanh:** Vinhomes — Quản lý đô thị thông minh

---

## 🏗️ Phase 3 — DEEP-DIVE

### 3.1. Current-State Workflow Mapping

Quy trình xử lý phản ánh cư dân hiện tại tại Ban Quản lý tòa nhà Vinhomes:

```
┌──────────────┐     ┌──────────────┐     ┌──────────────┐     ┌──────────────┐
│ Bước 1       │     │ Bước 2       │     │ Bước 3       │     │ Bước 4       │
│ Cư dân gửi  │     │ CSKH đọc &  │     │ 🔄 CSKH      │     │ Bộ phận tiếp │
│ phản ánh     │ ──→ │ phân loại   │ ──→ │ forward tới  │ ──→ │ nhận, lên    │
│ qua App      │     │ thủ công    │     │ đúng bộ phận │     │ lịch xử lý  │
│              │     │             │     │              │     │              │
│ Actor: Cư dân│     │ Actor: CSKH │     │ Actor: CSKH  │     │ Actor: Kỹ   │
│ ⏱ < 1 phút  │     │ ⏱ 8 phút 🔴 │     │ ⏱ 3 phút 🔴  │     │ thuật/VS sinh│
│ In: Văn bản  │     │ In: Văn bản │     │ In: Nhóm đã  │     │ ⏱ 8 phút    │
│ tự do        │     │ tự do       │     │ phân loại    │     │              │
│ Out: Ticket  │     │ Out: Nhóm   │     │ Out: Ticket  │     │ Out: Phản hồi│
│ chờ duyệt    │     │ danh mục    │     │ được route   │     │ cư dân       │
└──────────────┘     └──────────────┘     └──────────────┘     └──────────────┘

🔴 Bottleneck tại Bước 2 & 3
🔄 Handoff tại Bước 3 (CSKH → Bộ phận kỹ thuật / vệ sinh / an ninh)
⏱ Tổng thời gian xử lý thủ công: ~20 phút/lượt
   Vinhomes Hà Nội nhận ~300 phản ánh/ngày
```

---

### 3.2. Problem Statement (6-field)

| Field | Nội dung chi tiết |
|---|---|
| **1. Actor / Operator** | Nhân viên Chăm sóc Khách hàng (CSKH) tại Ban Quản lý tòa nhà Vinhomes — người đọc và phân loại thủ công từng phản ánh của cư dân mỗi ngày. |
| **2. Current Workflow** | Cư dân gửi phản ánh văn bản tự do qua App Vinhomes Resident → CSKH đọc từng ticket, tự phán đoán danh mục (kỹ thuật điện, nước, thang máy, vệ sinh, an ninh…) → forward thủ công sang group chat/email của bộ phận tương ứng → Bộ phận xử lý và phản hồi cư dân. Toàn bộ thủ công, dùng App nội bộ + Zalo/email, mất ~20 phút/lượt. |
| **3. Bottleneck** | Bước phân loại danh mục (Bước 2, ~8 phút): Văn bản phản ánh của cư dân rất đa dạng, mơ hồ ("nhà tôi có vấn đề" — không rõ điện hay nước). CSKH dễ nhầm danh mục → ticket bị route sai → bộ phận nhận phải forward lại → trễ SLA cam kết 2 giờ. |
| **4. Business Impact** | ~300 phản ánh/ngày × 11 phút xử lý thủ công (Bước 2+3) = **55 giờ nhân công/ngày** bị tiêu tốn chỉ để phân loại. Tỷ lệ route sai ước tính 15–20% → cư dân chờ quá 2 giờ → điểm hài lòng (NPS) giảm, ảnh hưởng trực tiếp đến tỷ lệ gia hạn hợp đồng thuê. |
| **5. Success Metric** | 1. **Tốc độ:** 90% ticket được phân loại và route tự động trong vòng dưới 30 giây (từ 8 phút thủ công).<br>2. **Độ chính xác:** Tỷ lệ phân loại đúng danh mục lần đầu ≥ 92% (đo qua xác nhận của bộ phận nhận ticket).<br>3. **SLA:** Tỷ lệ vi phạm SLA 2 giờ giảm từ 20% xuống dưới 5%. |
| **6. Operational Boundary** | **AI được phép:** Đọc nội dung phản ánh → phân loại danh mục → gắn mức độ ưu tiên (Khẩn/Thường) → tự động route ticket đến đúng bộ phận → draft phản hồi xác nhận gửi cư dân. **TUYỆT ĐỐI CẤM:** AI không được tự động đóng ticket; không được đưa ra cam kết thời hạn xử lý cụ thể mà không có nhân viên xác nhận; không xử lý các phản ánh liên quan đến tranh chấp pháp lý hoặc khiếu nại bồi thường (phải chuyển thẳng lên Trưởng BQL). **HITL bắt buộc:** Mọi phản hồi chính thức gửi cư dân phải có CSKH duyệt trước khi gửi. |

---

### 3.3. Future-State Flow & AI Fit

**AI Fit:** ✅ **LLM Feature** — Bài toán có input là văn bản tiếng Việt tự nhiên với ngữ nghĩa đa dạng, không thể giải bằng rule-based đơn thuần (keyword matching sẽ thất bại với các phản ánh mơ hồ hoặc đa vấn đề). Quy trình xử lý có cấu trúc cố định nên không cần Agent tự trị.

**So sánh AI Fit:**

| Phương án | Phù hợp? | Lý do |
|---|---|---|
| Rule / State-Machine | ❌ | Không xử lý được văn bản mơ hồ, đa danh mục; bảo trì keyword list tốn kém |
| **LLM Feature** | ✅ | Hiểu ngữ cảnh, phân loại chính xác kể cả phản ánh viết tắt hoặc không rõ ràng |
| Agentic Loop | ❌ | Overkill — quy trình có cấu trúc cố định, không cần agent tự ra quyết định đa bước |

**Quy trình tương lai (Future-State):**

```
┌──────────────┐     ┌──────────────┐     ┌──────────────┐     ┌──────────────┐
│ Bước 1       │     │ Bước 2       │     │ Bước 3       │     │ Bước 4       │
│ Cư dân gửi  │     │ 🔵 LLM phân  │     │ 🔵 Auto-route│     │ 🟢 CSKH      │
│ phản ánh     │ ──→ │ loại danh   │ ──→ │ ticket đến  │ ──→ │ duyệt draft  │
│ qua App      │     │ mục + mức   │     │ đúng bộ phận │     │ phản hồi &   │
│              │     │ độ ưu tiên  │     │ + draft phản │     │ gửi cư dân   │
│              │     │ < 30 giây   │     │ hồi xác nhận │     │              │
└──────────────┘     └──────────────┘     └──────────────┘     └──────────────┘
                                                                      │
                                                               ↩️ Fallback:
                                                               Nếu LLM confidence
                                                               thấp (< 0.7) hoặc
                                                               phát hiện từ khóa
                                                               "tranh chấp/bồi thường"
                                                               → Flag cho CSKH
                                                               xử lý thủ công.
```

- 🔵 **AI Step:** LLM đọc văn bản → phân loại danh mục + mức ưu tiên → auto-route
- 🟢 **Human Step (HITL):** CSKH duyệt bản draft phản hồi trước khi gửi cư dân
- ↩️ **Fallback:** Confidence score < 0.7 hoặc phát hiện nội dung pháp lý → chuyển tay CSKH

---

## 🏁 Phase 5 — EVALUATE

### AI Readiness Checklist:

1. ✅ **Chúng tôi có sẵn dữ liệu mẫu/logs sạch để test?**
   Hệ thống App Vinhomes Resident đã vận hành, lịch sử ticket phân loại thủ công từ CSKH là ground truth sẵn có để evaluate mô hình ngay từ ngày đầu.

2. ✅ **Rủi ro khi AI sai có nằm trong tầm kiểm soát (qua HITL hoặc Fallback)?**
   Mọi phản hồi chính thức gửi cư dân đều cần CSKH duyệt. Nếu AI route sai, bộ phận nhận sẽ re-route — không gây hậu quả an toàn hay pháp lý nghiêm trọng.

3. ✅ **Stakeholders sẵn sàng thay đổi quy trình làm việc cũ?**
   CSKH đang chịu tải nặng (~300 ticket/ngày), có động lực cao tiếp nhận công cụ hỗ trợ. BQL tòa nhà hưởng lợi trực tiếp từ việc giảm vi phạm SLA.

---

### Quyết định cuối cùng của Ban Giám Đốc Vin Smart Future:

**[x] GO — Bắt đầu xây dựng Prototype với scope hẹp.**

**Justification:**

> Bài toán đáp ứng đủ 3 điều kiện GO:
>
> **(1) Dữ liệu sẵn có** — Lịch sử ticket đã được CSKH phân loại thủ công tạo ground truth chất lượng cao để evaluate mô hình ngay từ ngày đầu, không cần giai đoạn thu thập dữ liệu kéo dài.
>
> **(2) Rủi ro kiểm soát được** — Sai sót của LLM chỉ dẫn đến ticket bị route sai và CSKH phải forward lại thủ công như trước — không gây hậu quả an toàn vật lý hay pháp lý. Fallback về quy trình cũ luôn khả thi.
>
> **(3) ROI rõ ràng** — 55 giờ nhân công/ngày đang bị tiêu tốn chỉ để phân loại. Nếu tự động hóa 80% lượng ticket, giải phóng >40 giờ/ngày để CSKH tập trung xử lý các trường hợp phức tạp, tranh chấp, và cải thiện chất lượng phản hồi.
>
> **Scope MVP đề xuất:** Triển khai thử nghiệm tại **1 tòa nhà Vinhomes** (~50 ticket/ngày) trong **4 tuần** để đo độ chính xác phân loại thực tế, thu thập feedback CSKH và xác nhận mức giảm vi phạm SLA trước khi nhân rộng toàn hệ thống.

