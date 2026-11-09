# Problem Scan — Đề xuất cá nhân cho nhóm

Vai trò: thành viên phụ trách scoping và prompt prototype. Họ và tên: Đào Văn Hiệp.

Bối cảnh theo đề Lab 02; chưa khảo sát doanh nghiệp. Các thời gian và mục tiêu dưới đây là giả định cần kiểm chứng, không phải số liệu thực tế.

## SCAN qua 4 lenses

| # | Đơn vị | Lens | Bài toán / người chịu tác động |
|---|---|---|---|
| 1 | Xanh SM | Tốn thời gian; stakeholder pain | Điều phối viên tổng hợp báo cáo pin thấp và soạn hướng dẫn; tài xế chờ phản hồi. |
| 2 | VinFast | Lặp lại | Kế toán đối chiếu hóa đơn sạc và giao dịch để tìm khoản lệch. |
| 3 | Vinhomes | AI-upgrade; lặp lại | CSKH đọc phản ánh tự do rồi chuyển tới bộ phận điện, nước, vệ sinh. |
| 4 | Vinpearl / VinWonders | AI-upgrade | CSKH soạn lại câu trả lời về giờ hoạt động và điều kiện vé từ tài liệu đã duyệt. |
| 5 | Vinmec | Stakeholder pain; tốn thời gian | Nhân viên hành chính tìm giấy tờ thiếu trong hồ sơ hẹn khám, không liên quan chẩn đoán. |
| 6 | Xanh SM | Lặp lại | Nhân viên vận hành tổng hợp lý do hủy chuyến từ ghi chú tự do hằng tuần. |

## Quick Problem Card #1 — Hỗ trợ báo cáo pin thấp

- **Công ty / Actor:** Xanh SM / điều phối viên; tài xế chờ kết quả.
- **Bài toán:** Giảm thời gian tạo nháp phản hồi sự cố pin.
- **Workflow 5 bước:** nhận báo cáo → xác minh pin/vị trí → kiểm tra hỗ trợ → soạn và duyệt tin → bàn giao đội hỗ trợ.
- **Bottleneck:** tra cứu 5 phút và soạn/duyệt 4 phút, tổng 9/15 phút trong kịch bản giả định.
- **AI hỗ trợ:** tóm tắt, soạn nháp từ dữ liệu đã xác minh; rule giới hạn action, con người quyết định hành động.
- **Metric mục tiêu:** trung vị từ 15 xuống ≤5 phút/hồ sơ, p90 ≤8 phút; 100% hành động cần người duyệt; 0 đề xuất chạy xe khi pin <5% trong bộ test.
- **Quick architecture:** Rule + LLM Feature; không agent tự trị. Cần telemetry giả lập và quy trình được duyệt; prototype không có quyền gửi lệnh.

## Quick Problem Card #2 — Phân loại phản ánh cư dân

- **Công ty / Actor:** Vinhomes / CSKH.
- **Bài toán:** Đề xuất bộ phận xử lý phản ánh ngôn ngữ tự do.
- **Workflow 4 bước:** nhận phản ánh → xác định nhóm → chuyển bộ phận → xác nhận tiếp nhận.
- **Bottleneck:** đọc và phân loại, giả định 4/7 phút/hồ sơ.
- **AI hỗ trợ:** gợi ý nhãn trong danh mục cố định; khẩn cấp hoặc nhiều nhãn chuyển người.
- **Metric mục tiêu:** phân loại từ 4 xuống ≤1 phút; macro-F1 ≥0,90 trên 100 mẫu hai người gán nhãn; không tự đóng khiếu nại.
- **Quick architecture:** rule cho mã rõ ràng, LLM cho câu tự do. Không tự cam kết bồi thường. Cần danh mục bộ phận và phản ánh ẩn danh.

## Quick Problem Card #3 — Tổng hợp lý do hủy chuyến

- **Công ty / Actor:** Xanh SM / nhân viên phân tích vận hành.
- **Bài toán:** Tạo báo cáo nguyên nhân từ ghi chú ẩn danh.
- **Workflow 4 bước:** xuất dữ liệu → đọc/gán nhãn → đếm nhóm → kiểm tra và viết báo cáo.
- **Bottleneck:** giả định 60 phút đọc 100 ghi chú trong tổng 90 phút/báo cáo.
- **AI hỗ trợ:** phân loại nháp, trích ID nguồn; code đếm; người kiểm tra mẫu.
- **Metric mục tiêu:** từ 90 xuống ≤30 phút/100 ghi chú; macro-F1 ≥0,85; 100% kết luận có ID nguồn.
- **Quick architecture:** LLM Feature + phép đếm xác định. SQL/rule đủ tốt nếu đã có mã nguyên nhân chuẩn.

## Đề xuất để nhóm review

Chọn card #1 vì phạm vi tương thích starter code, có thể kiểm thử bằng dữ liệu giả lập. Card #2 cần danh mục nghiệp vụ; card #3 có thể chỉ cần SQL. Đây là đề xuất cá nhân, chưa phải quyết định tập thể. Nếu template đạt chất lượng tương đương LLM, ưu tiên template để giảm chi phí và độ trễ.
