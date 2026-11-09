# 02. Báo Cáo Phân Tích Kỹ Thuật Chi Tiết (Deep-Dive Report)
Đơn vị thực hiện: Vin Smart Future
Dự án: Trợ lý điều phối bảo trì kỹ thuật đô thị Vinhomes Resident

## 1. Sơ đồ quy trình hiện tại (Current-State Workflow Mapping)

┌──────────────┐     ┌──────────────┐     ┌──────────────┐     ┌──────────────┐
│ Bước 1       │     │ Bước 2       │     │ Bước 3       │     │ Bước 4       │
│ Cư dân gửi   │     │ Lễ tân đọc   │     │ Lễ tân lập   │     │ Tổ trưởng    │
│ phản ánh lỗi │ ──> │ và phân loại │ ──> │ phiếu giấy   │ ──> │ giao việc    │
│ qua App      │     │ thủ công     │     │ bàn giao     │     │ cho thợ      │
│ ⏱ 2 phút    │     │ ⏱ 20 phút    │     │ ⏱ 15 phút   │     │ ⏱ 5 phút    │
│ In: Tin nhắn │     │ In: Text/Ảnh │     │ In: Dữ liệu  │     │ In: Phiếu    │
│ Out: Yêu cầu │     │ Out: Tên tổ  │     │ Out: Phiếu   │     │ Out: Phân công
└──────────────┘     └──────────────┘     └──────────────┘     └──────────────┘

Tổng thời gian xử lý thủ công: 42 phút mỗi yêu cầu.
    Bottleneck: Lễ tân không có chuyên môn kỹ thuật, mất 20 phút để đọc và đoán sai tổ thợ.
    Handoff: Bàn giao phiếu giấy giữa Lễ tân sảnh và Ban Kỹ thuật tầng hầm mất 15 phút.

## 2. Bản đặc tả bài toán (Problem Statement 6-field)

| Trường thông tin | Nội dung chi tiết |
|---|---|
| **1. Actor / Operator** | Nhân viên lễ tân sảnh và Tổ trưởng kỹ thuật tòa nhà Vinhomes. |
| **2. Current Workflow** | Cư dân gửi tin nhắn tự do kèm ảnh mô tả hư hỏng qua ứng dụng Vinhomes Resident. Lễ tân đọc tin nhắn, tra sổ tay để xếp loại vào tổ điện, nước hoặc xây dựng, sau đó viết phiếu bàn giao cho ban kỹ thuật. Quy trình thủ công mất trung bình 42 phút. |
| **3. Bottleneck** | Bước phân loại và lập phiếu (mất 35 phút). Lễ tân phân công sai tổ chuyên môn ở mức 30%, dẫn đến việc thợ đến hiện trường nhưng không đúng chuyên môn hoặc mang sai vật tư. |
| **4. Business Impact** | Mỗi tòa nhà tiếp nhận trung bình 120 yêu cầu mỗi ngày. Thời gian chờ đợi kéo dài khiến cư dân không hài lòng. Chi phí nhân công cho việc di chuyển kiểm tra nhầm tổ thợ tốn kém khoảng 180 giờ lao động mỗi tháng cho mỗi cụm chung cư. |
| **5. Success Metric** | 1. Giảm thời gian từ khi nhận phản ánh đến khi phát phiếu cho thợ từ 42 phút xuống dưới 3 phút. Tỷ lệ phân loại đúng tổ kỹ thuật và đề xuất đúng vật tư cơ bản đạt từ 92% trở lên. |
| **6. Operational Boundary** | Mô hình AI được phép trích xuất văn bản, ảnh chụp, gán mã tổ thợ và tạo bản nháp phiếu sửa chữa có gắn thẻ [DRAFT_ONLY]. Tuyệt đối không được tự động xuất lệnh điều động thợ vào nhà cư dân mà không có sự xác nhận của Tổ trưởng kỹ thuật. Khi phát hiện các từ khóa cháy nổ, chập điện bốc khói hoặc rò rỉ khí gas, hệ thống phải kích hoạt lệnh báo động khẩn cấp. |

## 3. Luồng vận hành tương lai và Mức độ tích hợp AI (Future-State Flow & AI Fit)

* **AI Fit:** Giải pháp thuộc nhóm **LLM Feature**. Cấu trúc bài toán có quy trình nghiệp vụ rõ ràng, không cần tác tử tự trị độc lập để tránh rủi ro an ninh trong khu căn hộ.
* **Quy trình tương lai:**

┌──────────────┐     ┌──────────────┐     ┌──────────────┐     ┌──────────────┐
│ Bước 1       │     │ Bước 2       │     │ Bước 3       │     │ Bước 4       │
│ Cư dân gửi   │     │ 🔵 AI bóc    │     │ 🟢 Tổ trưởng │     │ Thợ nhận    │
│ tin nhắn/ảnh │ ──> │ tách lỗi &   │ ──> │ kỹ thuật bấm │ ──> │ việc trên    │
│ qua App      │     │ tạo [DRAFT]  │     │ duyệt phiếu  │     │ App nội bộ   │
└──────────────┘     └──────────────┘     └──────────────┘     └──────────────┘
                            │                    │
                            ▼                    ▼
                     🔴 Cảnh báo cháy:      ↩️ Fallback:
                     Kích hoạt khẩn cấp     Nếu AI không chắc chắn,
                     chuông báo tòa nhà     phiếu chuyển sang lễ tân.

* **Phân định rõ các thành phần:**
  * 🔵 **AI Step:** Gemini trích xuất dữ liệu, phân loại nhóm thợ (Điện dân dụng, Cấp thoát nước, Mộc, Nhôm kính), tạo chuỗi JSON thông tin và gán nhãn `[DRAFT_ONLY]`.
  * 🟢 **Human Step (HITL):** Tổ trưởng kỹ thuật nhìn thấy phiếu nháp trên bảng điều khiển điều hành, kiểm tra độ hợp lý trong 10 giây và bấm nút xác nhận giao việc.
  * 🔴 **Ranh giới an toàn khẩn cấp:** Nếu tin nhắn chứa thông tin nguy hiểm (cháy, nổ, rò rỉ khí gas), hệ thống kích hoạt cấu trúc `dispatch_emergency_security` thông báo trực tiếp cho lực lượng an ninh trực sảnh trong vòng 5 giây.
  * ↩️ **Fallback:** Nếu mô hình trả về độ tin cậy dưới 75% hoặc lỗi mạng quá 3 giây, hệ thống chuyển nguyên văn tin nhắn của cư dân sang hàng đợi xử lý thủ công của lễ tân.

## 4. Đánh giá tính sẵn sàng và Quyết định triển khai (Evaluate)

### AI Readiness Checklist:
1. [x] Dữ liệu mẫu/logs sạch: Đã có sẵn 15.000 phiếu sửa chữa đã hoàn thành lưu trữ trên cơ sở dữ liệu của Vinhomes.
2. [x] Kiểm soát rủi ro: Đã thiết lập thẻ [DRAFT_ONLY], quy tắc phát hiện khẩn cấp và cơ chế duyệt bởi con người.
3. [x] Sự sẵn sàng của nhân sự vận hành: Đội ngũ kỹ thuật tòa nhà đã sử dụng điện thoại thông minh để nhận ca làm việc.

### Quyết định của Ban Giám Đốc Vin Smart Future:
[x] **GO (Bắt đầu xây dựng Prototype)**

**Lý giải quyết định:**
Dự án có bài toán rõ ràng, giảm thiểu chi phí vận hành đo đếm được cụ thể bằng giờ công của kỹ thuật viên. Rủi ro vận hành ở mức thấp nhờ giữ quy trình duyệt của tổ trưởng kỹ thuật trước khi gõ cửa căn hộ của cư dân. Chi phí gọi Gemini API định dạng JSON văn bản nhỏ hơn rất nhiều so với khoản tiết kiệm nhân sự hàng tháng.