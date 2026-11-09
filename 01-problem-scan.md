# Bài cá nhân — Scan và Quick Problem Cards

**Bối cảnh:** Vin Smart Future. **Người làm:** [Điền họ tên và mã sinh viên].  
**Lưu ý về dữ liệu:** Các con số trong tài liệu này là **giả định để thiết kế thí điểm**, không phải số liệu vận hành đã được Vingroup/Xanh SM/Vinhomes công bố hay xác nhận. Trước khi đề xuất triển khai, cần phỏng vấn người vận hành và đo baseline trên log thật.

## Hai bài toán doanh nghiệp đề xuất

1. **Xanh SM — Hỗ trợ điều phối sự cố pin xe điện ngoài hiện trường.** Điều phối viên phải đọc mô tả của tài xế, kiểm tra pin/vị trí/trạm sạc rồi quyết định hướng xử lý. Chậm hoặc sai ở bước đánh giá có thể dẫn tới chỉ đường không an toàn. Đề xuất AI chỉ **soạn phương án nháp** và phát hiện tình huống pin dưới 5%; điều phối viên xác minh dữ liệu và ra quyết định cuối.
2. **Vinhomes — Phân loại và chuyển phản ánh cư dân tới đúng bộ phận.** Nhân viên CSKH đọc nội dung tự do, đối chiếu tòa nhà và loại sự cố rồi chuyển việc. Phản ánh thiếu thông tin hoặc chuyển sai tổ làm tăng thời gian xử lý. Đề xuất LLM phân loại, trích thông tin và soạn nháp phiếu; nhân viên duyệt trước khi giao việc.

Hai đề xuất này đại diện cho hai mức rủi ro khác nhau. Bài toán Xanh SM ưu tiên an toàn vận hành, phù hợp cho bài prototype bắt buộc của repo. Bài toán Vinhomes phù hợp làm thí điểm xử lý văn bản với ranh giới rõ.

## Phase 1 — SCAN: 5 cơ hội cụ thể

| # | Công ty | Lens chính | Bài toán/bottleneck quan sát cần xác minh | Actor và tín hiệu đo |
|---|---|---|---|---|
| 1 | Xanh SM | Stakeholder Pain | Khi tài xế báo pin thấp ngoài hiện trường, điều phối viên phải đọc tin nhắn và tự đối chiếu pin, vị trí, trạm sạc; đề xuất trạm quá xa có thể gây rủi ro. | Điều phối viên, tài xế; đo thời gian từ báo sự cố đến phương án được duyệt và số gợi ý sai ranh giới. |
| 2 | Vinhomes | Lặp lại | CSKH phải đọc từng phản ánh cư dân và chọn tổ xử lý; phản ánh thiếu tòa nhà/loại sự cố bị chuyển qua lại. | CSKH, tổ kỹ thuật; đo thời gian phân loại và tỷ lệ chuyển sai tổ. |
| 3 | Xanh SM | Tốn thời gian | Tài xế mô tả điểm đón bằng tiếng Việt tự do; điều phối viên phải gọi lại để đối chiếu với tọa độ ứng dụng khi địa chỉ không khớp. | Điều phối viên, tài xế, khách; đo số cuộc gọi xác minh và thời gian xác định điểm đón. |
| 4 | Vinhomes | AI có thể tốt hơn | Mẫu phản hồi cư dân thường chung chung; CSKH mất thời gian đọc lịch sử phiếu để soạn câu trả lời đúng ngữ cảnh. | CSKH, cư dân; đo thời gian soạn phản hồi và số lần phải sửa trước khi gửi. |
| 5 | Xanh SM | Lặp lại | Điều phối viên tổng hợp thủ công ghi chú về các chuyến bị hủy để phân loại nguyên nhân và báo cáo cuối ca. | Điều phối viên, quản lý ca; đo thời gian tổng hợp và tỷ lệ ghi chú không phân loại được. |

**Cách xác minh scan:** phỏng vấn ít nhất một điều phối viên/nhân viên CSKH nếu có thể tiếp cận; lấy mẫu phiếu/tin nhắn đã ẩn danh; đo 20–50 lượt xử lý. Nếu không có quyền truy cập dữ liệu, giữ các mục trên là giả thuyết và nêu hạn chế trong quyết định cuối.

## Phase 2 — QUICK-ASSESS: 3 cards

### Card 1 — Xanh SM: Sự cố pin ngoài hiện trường (ưu tiên số 1)

| Trường | Nội dung |
|---|---|
| Bài toán 1 câu | Điều phối viên cần đề xuất xử lý nhanh và an toàn khi tài xế báo xe điện sắp hết pin ngoài hiện trường. |
| Công ty / Actor | Xanh SM; điều phối viên và tài xế. |
| Workflow hiện tại | 1. Tài xế báo pin và vị trí → 2. Điều phối viên đọc tin/kiểm tra dữ liệu → 3. Tra trạm sạc hoặc phương án cứu hộ → 4. Trao đổi với tài xế → 5. Điều phối viên duyệt và liên hệ đội hỗ trợ. |
| Bottleneck | Bước 2–3: đối chiếu nhiều nguồn dữ liệu, dễ bỏ sót ngưỡng pin. **Giả định** 6–10 phút/lượt; cần đo bằng log. |
| AI hỗ trợ | Tóm tắt tình huống, phát hiện pin <5%, tạo JSON nháp `dispatch_mobile_charger`; không tự gửi/lên lệnh. |
| Metric có số | Mục tiêu thử nghiệm: thời gian tạo phương án nháp <2 phút/lượt trên 50 ca giả lập/ẩn danh; **0/50** gợi ý trạm >5 km khi pin <5%. Baseline cần đo trước khi kết luận cải thiện. |
| Quick Architecture | **LLM feature + rule cứng** kiểm tra pin/khoảng cách + người duyệt. Không cần agent tự hành động. |
| Ranh giới/fallback | Nếu thiếu pin, vị trí hoặc trạm chưa được xác minh: chuyển `needs_human_review`; điều phối viên gọi tài xế. |

### Card 2 — Vinhomes: Phân loại phản ánh cư dân (ưu tiên số 2)

| Trường | Nội dung |
|---|---|
| Bài toán 1 câu | CSKH cần phân loại phản ánh bằng ngôn ngữ tự nhiên và chuyển tới đúng tổ xử lý ngay từ lần đầu. |
| Công ty / Actor | Vinhomes; nhân viên CSKH, tổ kỹ thuật và cư dân. |
| Workflow hiện tại | 1. Cư dân gửi phản ánh → 2. CSKH đọc và tìm tòa nhà/căn hộ → 3. Chọn nhóm sự cố → 4. Chuyển tổ phụ trách → 5. Tổ nhận việc hoặc trả lại nếu sai/thiếu thông tin. |
| Bottleneck | Bước 2–4: mô tả tự do, thiếu thông tin và có nhiều loại sự cố. **Giả định** 4–8 phút/phiếu; cần đo. |
| AI hỗ trợ | Trích tòa nhà, loại sự cố, mức khẩn cấp; soạn nháp phiếu và nêu thông tin còn thiếu. |
| Metric có số | Mục tiêu thử nghiệm: ≥85% trong 100 phiếu mẫu ẩn danh được đề xuất đúng tổ; thời gian phân loại trung vị <2 phút/phiếu sau khi có người duyệt. Cần đo baseline và gán nhãn chuẩn bởi CSKH. |
| Quick Architecture | **LLM feature** cho văn bản tự do; rule đối chiếu danh sách tòa nhà/tổ; CSKH duyệt chuyển việc. |
| Ranh giới/fallback | Không tự gửi thông báo/đóng phiếu; phiếu khẩn cấp, thiếu dữ liệu hoặc độ tin cậy thấp chuyển nhân viên xử lý trực tiếp. |

### Card 3 — Xanh SM: Xác minh điểm đón không khớp (để dự phòng)

| Trường | Nội dung |
|---|---|
| Bài toán 1 câu | Điều phối viên cần xác định điểm đón khi mô tả của tài xế/khách khác tọa độ ứng dụng. |
| Công ty / Actor | Xanh SM; điều phối viên, tài xế và khách. |
| Workflow hiện tại | 1. Phát sinh lệch điểm đón → 2. Đọc tin nhắn → 3. So với tọa độ bản đồ → 4. Gọi xác minh → 5. Cập nhật điểm đón sau khi xác nhận. |
| Bottleneck | Bước 2–4: địa danh mơ hồ, phải gọi lại. **Giả định** 3–7 phút/lượt; cần đo. |
| AI hỗ trợ | Tách địa danh/mốc từ tin nhắn và soạn câu hỏi xác minh; không tự đổi tọa độ chuyến. |
| Metric có số | Mục tiêu thử nghiệm: giảm số lượt gọi xác minh trung bình từ baseline đo được xuống tối đa 1 lượt/ca trên 50 ca mẫu; không có lần tự ý đổi điểm đón. |
| Quick Architecture | **Rule + LLM feature** để đọc mô tả; người xác nhận vị trí. |
| Ranh giới/fallback | Nếu nhiều địa điểm cùng tên hoặc GPS sai lệch, yêu cầu người xác minh qua điện thoại. |

## Quyết định đề xuất cho nhóm

Chọn **Card 1** để deep dive và làm prompt prototype vì ranh giới an toàn đo được, phù hợp [starter code](./starter-code/prompt_prototype.py) và có tình huống adversarial rõ. **Card 2** là phương án doanh nghiệp thứ hai để nhóm cân nhắc nếu có dữ liệu phiếu phản ánh ẩn danh và nhân viên gán nhãn. **Card 3** dự phòng vì cần dữ liệu bản đồ/GPS và quy trình xác minh thực địa khó tiếp cận hơn.

Sơ đồ quy trình **hiện tại** cho Card 1 nằm tại [04-workflow-diagram.png](./04-workflow-diagram.png). Sơ đồ dùng tổng thời gian giả định 8 phút/ca (1 + 2 + 3 + 1 + 1), phù hợp baseline minh họa trong phần ước tính lợi ích bên dưới; nhóm cần thay bằng thời gian đo được nếu có dữ liệu thật.

**Bước tiếp theo trước khi gọi đây là đề xuất triển khai thật:** xác nhận workflow với người vận hành, lấy dữ liệu hợp lệ, đo baseline và kiểm chứng giả định rủi ro/chi phí. Nếu không có dữ liệu, kết luận hợp lý ở Phase 5 có thể là **NOT YET**.

## Ước tính lợi ích doanh nghiệp — kịch bản minh họa

**Công thức:** `Giờ tiết kiệm/tháng = số ca mỗi ngày × số ngày vận hành × (phút hiện tại − phút sau cải tiến) / 60`. `Giá trị năng lực xử lý = giờ tiết kiệm × chi phí lao động giả định/giờ`. Đây là **giá trị thời gian được giải phóng**, chưa phải tiền mặt hoặc doanh thu tăng thực tế. Mọi đầu vào bên dưới đều là **giả định để luyện scoping**, không phải dữ liệu của Xanh SM hay Vinhomes.

| Bài toán | Giả định đầu vào | Thời gian tiết kiệm | Giá trị năng lực xử lý giả định | Lợi ích vận hành khác cần kiểm chứng |
|---|---|---:|---:|---|
| Xanh SM — sự cố pin | 20 ca/ngày × 26 ngày/tháng; từ 8 xuống 2 phút/ca; 70.000 đ/giờ điều phối | **52 giờ/tháng**, tương đương 624 giờ/năm | **3,64 triệu đ/tháng**, tương đương 43,68 triệu đ/năm | Phản hồi sớm hơn; giảm gợi ý trạm không an toàn. Mục tiêu thử: 0/50 ca pin <5% bị gợi ý đi trạm >5 km. |
| Vinhomes — phân loại phản ánh | 100 phiếu/ngày × 26 ngày/tháng; từ 6 xuống 2 phút/phiếu, đã tính bước nhân viên duyệt; 60.000 đ/giờ CSKH | **173,3 giờ/tháng**, tương đương 2.080 giờ/năm | **10,4 triệu đ/tháng**, tương đương 124,8 triệu đ/năm | Ít phiếu chuyển sai tổ hơn, phản hồi cư dân nhanh hơn. Mục tiêu thử: ≥85/100 phiếu đề xuất đúng tổ. |

**Ví dụ tính Xanh SM:** `20 × 26 × (8 − 2) / 60 = 52 giờ/tháng`; `52 × 70.000 = 3.640.000 đ/tháng`. **Ví dụ tính Vinhomes:** `100 × 26 × (6 − 2) / 60 = 173,3 giờ/tháng`; `173,3 × 60.000 ≈ 10.400.000 đ/tháng` (dùng giá trị chưa làm tròn để nhân).

Không cộng các số trên thành “doanh thu tăng”. Doanh thu chỉ tăng nếu thời gian xe/tài xế được giải phóng thật sự chuyển thành thêm chuyến có lãi, hoặc dịch vụ cư dân tạo thêm nguồn thu có thể đo. Tiền mặt chỉ tiết kiệm nếu doanh nghiệp giảm được tăng ca, thuê ngoài hay chi phí khác; còn nếu không, đây là **năng lực xử lý được giải phóng**. Khi tính ROI cần trừ chi phí API, tích hợp, bảo mật, rà soát của con người và sửa lỗi.

Vinhomes [đã công bố trợ lý ảo trên ứng dụng Vinhomes Resident](https://vinhomes.vn/vi/ra-mat-tro-ly-ao-tren-ung-dung-vinhomes-resident-va-vinhomes-online), vì vậy đề xuất ở đây nên tập trung vào **phân loại và chuyển phiếu phản ánh trong quy trình CSKH**; cần khảo sát chức năng hiện có trước khi xây thêm. Trang chính thức của Vinhomes cũng mô tả ứng dụng Resident là kênh kết nối cư dân với ban quản lý, nhưng **không cung cấp các số ca, thời gian và chi phí** dùng trong bảng trên.

**Dữ liệu cần thu để thay giả định:** số ca/phiếu mỗi ngày theo từng cơ sở; thời điểm bắt đầu/kết thúc từng bước; tỷ lệ chuyển sai và xử lý lại; chi phí lao động đầy đủ; chi phí API/tích hợp; chất lượng đầu ra trên tập ca đã ẩn danh. Chạy thí điểm song song với người duyệt, so sánh ít nhất 50 ca Xanh SM và 100 phiếu Vinhomes trước khi kết luận có lợi ích thực tế.
