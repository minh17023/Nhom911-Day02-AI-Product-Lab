# 03 — AI Log & Reflection

**Nhóm:** Nhom911
**Người thực hiện:** Nguyễn Hữu Thịnh — MSSV 2A202603726 — 26ai.thinhnh2@vinuni.edu.vn

**Thành viên nhóm:**

| Họ và tên | Mã sinh viên | Email | Vai trò |
|---|---|---|---|
| Nguyễn Hữu Thịnh | 2A202603726 | 26ai.thinhnh2@vinuni.edu.vn | Thành viên |

**Công cụ AI đã dùng:**
- **Claude** (Claude Code trong VS Code): thought-partner xuyên suốt buổi lab — đọc đề, lập kế hoạch, viết code, soạn tài liệu, phản biện.
- **Gemini** (qua API): mô hình được đặt ranh giới và bị "tấn công" trong prompt prototype.

> Bản phản ánh này được soạn với sự hỗ trợ của Claude, dựa trên các sự kiện thật đã diễn ra trong buổi làm.

---

## 1. Cách tôi làm việc với AI hôm nay

Tôi bắt đầu buổi lab với một tài liệu ý tưởng đã viết sẵn về hai bài toán: **Vinmec phân luồng triệu chứng** và **Vinhomes tiếp nhận phản ánh cư dân**. Tài liệu khá đầy đủ về kiến trúc, ranh giới và test case, nhưng tôi chưa biết nó khớp với yêu cầu chấm điểm đến đâu.

Vì vậy tôi dùng Claude theo trình tự:
1. Đọc toàn bộ repo và file PDF đề bài, giải thích mục tiêu và yêu cầu.
2. Đối chiếu tài liệu ý tưởng của tôi với README, chỉ ra phần còn thiếu.
3. Triển khai từng bước. **Sau mỗi bước phải tự kiểm duyệt** (chạy code, chạy autograder, đối chiếu từng câu trong báo cáo với output thật) rồi mới sang bước tiếp theo.
4. Đặt câu hỏi ngược lại để kiểm chứng: "gọi API liên quan gì tới phần đã làm, đưa bằng chứng bằng số", "đánh giá bài toán theo thời gian và tiền bạc", "đọc lại README xem đã đúng hết chưa".

Yêu cầu "kiểm duyệt từng bước" và "đưa bằng chứng bằng số" là hai quyết định giúp tôi nhiều nhất. Phần lớn lỗi dưới đây được phát hiện nhờ chúng.

---

## 2. AI đã giúp tôi những gì

**Hiểu luật chơi trước khi làm.** Claude đọc file `autograder.py` và chỉ ra điều tôi không để ý: máy chấm kiểm tra **từ khóa cứng** của bài Xanh SM (`draft_only`, `5%`, `dispatch_mobile_charger`), đếm chữ "Failed" trong **toàn bộ output** (kể cả câu trả lời của model) và chỉ chờ **30 giây**. Nhờ vậy tôi không đổi file chấm sang bài Vinhomes (sẽ mất khoảng 2/5 điểm), mà tạo một file prototype riêng `vinhomes_prototype.py`.

**Tìm yêu cầu bị ẩn trong slide.** Đọc PDF 21 trang, Claude phát hiện hai yêu cầu mà README không nhắc: **đầu mỗi file phải ghi tên nhóm, họ tên và email thành viên**, và **Python phải lớn hơn 3.11 và nhỏ hơn 3.14** (máy tôi đang dùng 3.14).

**Chỉ ra phần còn thiếu ở những mục nhiều điểm nhất.** Tài liệu ý tưởng của tôi mạnh về kiến trúc, nhưng thiếu đúng ba phần chiếm 50/100 điểm nhóm: **thời gian từng bước** của quy trình hiện tại (G1, 20 điểm), **bảng Problem Statement đúng 6 trường** (G2, 20 điểm) và **quyết định GO / NOT YET / NO-GO** (G4, 10 điểm).

**Phản biện như một CFO.** Khi bị hỏi "phân loại 6 bộ phận bằng từ khóa là đủ, cần gì LLM?" và "con số 30% chuyển sai lấy ở đâu?", tôi phải tách rõ phần nào thật sự cần LLM (đọc hiểu văn bản tự do) và phần nào nên là code (khẩn cấp, SLA, gộp trùng). Tôi cũng phải thừa nhận các con số của mình là giả định.

**Biến thiết kế thành bằng chứng.** Thay vì chỉ mô tả 8 test case trên giấy, tôi có kết quả chạy thật: **8/8 test Vinhomes đạt**, **3/3 test Xanh SM đạt**, autograder **10.00/10.00**.

**Lượng hóa giá trị.** Claude giúp tôi dựng bài toán thời gian và chi phí. Với 300 phản ánh/ngày/khu, hệ thống tiết kiệm **24–40 giờ công/ngày**, tương đương **41–69 triệu đồng/tháng**. Chi phí gọi AI (từ số token đo thật, khoảng 620 token/lần gọi) chỉ khoảng **0,5 triệu/tháng**. Điểm hòa vốn là chỉ cần tiết kiệm **2 phút/phản ánh**.

---

## 3. AI đã sai ở đâu

**1. Hướng dẫn sai môi trường.** Claude đưa lệnh PowerShell (`.venv\Scripts\Activate.ps1`) trong khi tôi dùng **Git Bash**. Tôi gõ `pip install` và nhận lỗi `pip: command not found`, rồi tưởng do Python 3.14. Nguyên nhân thật là tôi chưa kích hoạt môi trường ảo. Lệnh đúng trong Git Bash là `source .venv/Scripts/activate`.

**2. Trả lời khi chưa đọc tài liệu gốc.** Khi tôi hỏi "có phải do Python 3.14 không", Claude trả lời "không" (đúng với riêng lỗi `pip`). Chỉ sau khi tôi yêu cầu đọc file PDF, nó mới thấy đề bài yêu cầu Python **nhỏ hơn 3.14**. Câu trả lời đầu đúng về chi tiết nhưng thiếu bối cảnh.

**3. Code theo đề không còn chạy được, và mất nhiều vòng mới sửa xong.** Model `gemini-2.5-flash` mà đề bài chỉ định trả về **lỗi 404**: API key tạo mới không còn được dùng model này. Các lần sửa lần lượt là:

| Lần | Thử | Kết quả |
|---|---|---|
| 1 | Chuyển sang `gemini-3.8-flash` | Model quá tải (503), một lần chạy **treo hơn 120 giây** |
| 2 | Dùng `gemini-3.5-flash` | Chạy **28,9 giây**, sát giới hạn 30 giây. Lần chạy autograder sau đó **bị timeout, chỉ đạt 3/5** |
| 3 | Đặt giới hạn 8 giây/lần gọi | API báo **tối thiểu là 10 giây** |
| 4 | Đặt 10 giây | Lỗi **504** chưa được xử lý, vẫn không chuyển sang model dự phòng |
| 5 | Ưu tiên `gemini-3.5-flash-lite`, tắt tự gọi lại, bỏ qua model lỗi | 3 lần chạy liên tiếp khoảng **5 giây**, autograder **5.00/5.00** |

Bài học: một lần chạy thành công chưa phải là xong. Phải chạy lại nhiều lần và đo thời gian.

**4. AI viết một câu không có thật vào bài nộp.** Trong `01-problem-scan.md`, Claude viết "Đã dán từng thẻ vào LLM với prompt CFO", trong khi tôi chưa hề làm việc đó. Bước tự kiểm duyệt phát hiện ra, và câu được sửa thành "phản biện thực hiện cùng trợ lý AI khi soạn thẻ". Nếu không có bước kiểm duyệt, bài nộp của tôi đã chứa một khẳng định sai.

**5. Lộ API key trong log.** Để xem định dạng file `.env`, Claude chạy lệnh "che" giá trị key. Nhưng biểu thức che không khớp định dạng của tôi (`GEMINI_API_KEY = "..."` có dấu cách và key bắt đầu bằng `AQ.`), nên key bị in ra log công cụ trên máy.

**6. Số liệu không có nguồn.** Các con số 30% chuyển sai, 65% đạt SLA, 300 phản ánh/ngày, lương lễ tân 12 triệu/tháng đều là giả định. Riêng giá token của Gemini, Claude **không biết chính xác** và đã tự nói ra điều đó, dùng một mức giá giả định cao để tính. Đây là cách xử lý đúng. Tuy vậy, tôi vẫn phải chủ động ghi "giả định" ở mọi file.

**7. Gemini cũng mắc lỗi, dù test vẫn báo "đạt":**
- **H-06:** model điền `unit = "Hành lang tầng 3"`, đưa vị trí vào ô số căn.
- **H-04:** model gắn ticket vào căn 1205, tức là căn bị hỏi thông tin, không phải căn của người gửi.
- **Test 2 Xanh SM:** với `flash-lite`, model **từ chối** soạn tin chúc tài xế vì cho là "ngoài phạm vi" (over-refusal), dù vẫn giữ thẻ `[DRAFT_ONLY]`. Ở lần chạy với `3.5-flash`, model soạn tin bình thường.

---

## 4. Tôi đã sửa thế nào

| Vấn đề | Cách tôi xử lý |
|---|---|
| Lệnh sai môi trường | Nói rõ mình dùng Git Bash. Claude đưa bảng lệnh PowerShell ↔ Git Bash |
| AI trả lời khi chưa đọc đề | Yêu cầu **đọc file PDF đề bài trước** rồi mới giải thích. Lập bảng những chỗ slide khác README |
| Model bị chặn / chậm | Đo thời gian từng lần gọi để tìm đúng nguyên nhân rồi mới sửa: có model dự phòng, giới hạn 10 giây, tắt tự gọi lại |
| Câu khẳng định sai trong bài nộp | Yêu cầu **kiểm duyệt từng bước**, đối chiếu từng câu trong báo cáo với output thật |
| Báo cáo trích kết quả của model cũ | Chạy lại bằng đúng model đang dùng và cập nhật bảng. Nhờ vậy phát hiện thêm lỗi over-refusal |
| Không rõ API đóng góp gì | Yêu cầu bằng chứng bằng số. Chạy autograder với key thật và key giả: **5/5 so với 4/5**, khác nhau đúng ở tiêu chí "ranh giới thực sự được giữ" |
| Số liệu giả định | Ghi "giả định" ở đầu mọi file. Đưa "đo baseline thật từ 500 phản ánh cũ" thành bước đầu tiên của lộ trình và thành điều kiện dừng dự án |
| Lỗi trích xuất của Gemini (H-06) | **Không nới test cho dễ đạt**, mà đưa vào thiết kế: code kiểm tra tòa/căn với danh sách hợp lệ |
| Tôi lỡ xóa phần thông tin nhóm khi sửa file 01 | Rà soát lại theo README, phát hiện thiếu và thêm lại |
| Lộ key | Sẽ tạo API key mới sau buổi lab. `.env` đã nằm trong `.gitignore` nên không bị push |

---

## 5. Bài học

**AI là thought-partner, không phải nguồn sự thật.** Claude làm việc rất nhanh và có cấu trúc, nhưng có lúc khẳng định điều chưa xảy ra. Mọi câu trong bài nộp phải truy được về một bằng chứng: output, tài liệu, hoặc số đo.

**"Test đạt" chưa chắc là "đúng".** 8/8 test đạt, nhưng đọc kỹ output vẫn thấy lỗi trích xuất và lỗi từ chối nhầm. Test ranh giới phải kiểm tra **cả hai chiều**: AI có vi phạm không, và AI có từ chối quá mức không.

**Ranh giới phải nằm ở code, không chỉ ở prompt.** Prompt giữ được thẻ `[DRAFT_ONLY]` trong mọi lần thử. Nhưng lỗi H-06 cho thấy không thể tin tuyệt đối vào model. Những gì cần đúng tuyệt đối (khẩn cấp, SLA, dữ liệu cá nhân) phải do code đảm bảo.

**Đọc đề gốc trước khi hỏi AI.** Phần lớn sai sót đầu buổi đến từ việc cả AI và tôi suy luận khi chưa đọc slide.

**Môi trường thật khác môi trường trong đề.** Model trong đề không còn dùng được với tài khoản mới. Kỹ sư phải đo đạc và xử lý, không chỉ chép code mẫu.

**Phần quan trọng nhất không cần AI.** Theo phân tích bằng số, chỉ **1/10 điểm autograder** phụ thuộc vào việc gọi API thành công. Phần chiếm nhiều điểm nhất, **quy trình hiện tại và Problem Statement (40/100)**, là tư duy về bài toán. Điều này đúng với tinh thần của lab: **xác định bài toán trước, AI sau**.
