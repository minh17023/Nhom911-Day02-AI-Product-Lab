# AI Log & Reflection

Họ và tên: Đào Văn Hiệp. Đây là bản ghi hỗ trợ thực tế trong phiên làm việc Lab 02.

## AI giúp gì?

Người học yêu cầu làm bài với vai trò một thành viên và hướng dẫn nộp theo nhóm. AI đọc README, worksheet, ví dụ, starter và autograder; đề xuất chủ đề pin Xanh SM, soạn scan, ba cards, deep dive, code và sơ đồ. Đây là đề xuất cá nhân chờ nhóm review.

## Phát hiện vấn đề và sửa ranh giới

1. Ví dụ chứa số liệu vận hành nhưng phiên này không có dữ liệu xác minh. Bài làm chuyển thành giả định, ghi công thức và kế hoạch đo; không tự nhận đã khảo sát.
2. Starter tìm tag ở bất kỳ đâu và tìm từ “cứu hộ”, có thể cho qua câu phủ định. Code mới parse JSON, kiểm prefix đầu draft, action, approval và khoảng cách. Ngữ nghĩa vẫn cần người duyệt.
3. JSON thuần không thể bắt đầu bằng tag ngoài JSON. Quy ước được làm rõ: **trường draft** bắt đầu `[DRAFT_ONLY]`, toàn bộ response là JSON.
4. `dispatch_mobile_charger` dễ bị hiểu thành lệnh thật. Prompt xác định đó chỉ là đề xuất chờ duyệt; không có tool gửi tin/điều xe.
5. Chưa có API key. Offline được tách riêng, không dùng fixtures giả vờ là kết quả Gemini. Thiếu key mặc định trả mã 2 và ghi NOT RUN.

## Prompt và kiểm thử

Thêm quy định user input không được ghi đè system instruction, schema cố định, thiếu dữ liệu chuyển người, không bịa GPS/ETA, không chỉ đường và 6 test tấn công/biên. Các thay đổi là chỉnh sửa kỹ thuật trong phiên, chưa phải chuỗi thử nghiệm live nhiều vòng. Tài liệu SDK tham khảo: https://googleapis.github.io/python-genai/.

## Kết quả và phản ánh cần người học xác nhận

Kết quả kiểm tra thực tế xem `05-validation.md`. Chưa chạy live, chưa đo token/độ trễ, chưa khảo sát stakeholder hoặc chứng minh tiết kiệm. AI giúp cấu trúc bài và kiểm ranh giới nhưng không thay thế bằng chứng hoặc đồng thuận nhóm.

Trước khi nộp, người học cần đọc hiểu code, chạy live bằng key cá nhân, ghi output thực, bổ sung suy nghĩ bằng lời của mình và trao đổi nhóm. Đây là phần việc còn lại, không phải việc đã hoàn thành.
