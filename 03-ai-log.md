# Tên nhóm: Nhóm 911
# Thành viên:
# - [Họ và tên] | [email@example.com]

---

# 03 — AI Log: Nhật ký tương tác & Phản ánh cá nhân

> **Lab 02: AI Product Scoping — Vin Smart Future**
> Ghi lại quá trình sử dụng AI (Gemini, ChatGPT, Claude...) làm thought-partner trong buổi lab.

---

## 1. AI đã giúp được gì?

Trong buổi lab, tôi sử dụng AI (Gemini) làm đồng hành tư duy xuyên suốt quá trình scoping bài toán.

**Brainstorm bài toán (Phase 1 — SCAN):**
Tôi prompt: *"Tôi là AI Engineer tại Vin Smart Future. Tìm các pain point vận hành cụ thể tại Vinhomes có thể tối ưu bằng AI, kèm con số tổn thất ước tính."* AI trả về danh sách 7 bài toán rất nhanh, trong đó bài toán phân loại phản ánh cư dân và bài toán chẩn đoán lỗi xe VinFast đều xuất hiện với số liệu ước tính cụ thể. Điều này giúp tôi nhanh chóng định hướng thay vì mất nhiều thời gian tìm kiếm từ đầu.

**Stress-test thẻ bài toán (Phase 2 — QUICK-ASSESS):**
Tôi dán nội dung Quick Problem Card #1 (Vinhomes CSKH) vào Gemini và yêu cầu: *"Đóng vai CFO cực kỳ khắt khe, chỉ ra 3 điểm yếu và tại sao rule-based có thể tốt hơn AI."* AI phản biện rất sắc: chỉ ra rằng keyword matching có thể xử lý được 70% phản ánh phổ biến với chi phí thấp hơn nhiều, và đặt câu hỏi về độ chính xác của LLM với các phản ánh viết tắt kiểu "nhà t vđề đèn". Phản biện này giúp tôi làm rõ hơn phần Operational Boundary và Fallback trong Problem Statement.

**Viết Problem Statement 6-field (Phase 3):**
AI hỗ trợ cụ thể hóa Business Impact bằng cách tính toán: 300 ticket/ngày × 11 phút = 55 giờ nhân công/ngày. Con số cụ thể này thuyết phục hơn nhiều so với chỉ nói "mất nhiều thời gian".

---

## 2. AI đã trả lời sai / hallucinate ở đâu?

**Hallucination về số liệu thực tế:**
Khi hỏi *"Vinhomes Hà Nội nhận bao nhiêu phản ánh mỗi ngày?"*, AI tự tin đưa ra con số "khoảng 500–800 phản ánh/ngày" nhưng không có nguồn tham chiếu. Tôi không thể xác minh con số này, nên đã điều chỉnh xuống ~300 (ước tính bảo thủ hơn) và ghi rõ đây là con số ước tính trong Problem Statement.

**Gợi ý "Agentic Loop" không phù hợp:**
Ban đầu AI gợi ý dùng Agentic Loop vì nghĩ bài toán cần AI "tự quyết định nhiều bước". Nhưng sau khi tôi phân tích lại workflow, quy trình xử lý ticket có cấu trúc cố định 4 bước — LLM Feature là đủ và an toàn hơn. AI đã "oversell" độ phức tạp cần thiết.

**Metric không có ngưỡng rõ ràng:**
AI draft metric ban đầu là *"cải thiện tốc độ xử lý"* — quá mơ hồ. Tôi phải prompt lại: *"Đưa ra metric dạng SLA với ngưỡng số cụ thể, có thể đo được trong vòng 4 tuần thử nghiệm."* Lúc đó AI mới ra metric hữu dụng: "90% ticket phân loại dưới 30 giây, độ chính xác ≥ 92%".

---

## 3. Tôi đã sửa prompt / ranh giới ra sao?

| Vấn đề gặp phải | Cách tôi điều chỉnh |
|---|---|
| AI đưa số liệu không có nguồn | Thêm vào prompt: *"Chỉ đưa số liệu có nguồn tham chiếu rõ ràng hoặc ghi rõ là ước tính"* |
| AI gợi ý giải pháp quá phức tạp | Thêm ràng buộc: *"Ưu tiên giải pháp đơn giản nhất có thể triển khai trong 4 tuần"* |
| Metric quá chung chung | Prompt cụ thể hơn: *"Metric phải có dạng: X% [hành động] trong vòng [thời gian] đạt ngưỡng [con số]"* |
| AI bỏ qua Operational Boundary | Nhắc lại: *"Phần Boundary phải nêu rõ AI được phép làm gì và TUYỆT ĐỐI CẤM làm gì"* |

---

## 4. Bài học rút ra

Dùng AI làm thought-partner hiệu quả nhất khi tôi **đặt ra ràng buộc rõ ràng và phản biện ngược lại** những gì AI đưa ra. AI giỏi brainstorm nhanh và viết có cấu trúc, nhưng dễ hallucinate số liệu và có xu hướng "oversell" giải pháp phức tạp. Vai trò của người dùng là giữ vững tư duy phê phán (critical thinking) và dùng AI như một công cụ tăng tốc — không phải nguồn đáp án cuối cùng.

