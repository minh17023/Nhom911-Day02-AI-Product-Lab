# Hướng dẫn nộp Lab 02 theo nhóm

Theo quy định trong README của đề: nhóm 4–6 người; tất cả thành viên kể cả trưởng nhóm có nhánh riêng. Code cá nhân được chấm trên nhánh cá nhân; nhóm chọn lọc tài liệu vào main. Chỉ trưởng nhóm điền form chính thức.

## 1. Tình trạng thư mục hiện tại

Đã soạn phần bài của một thành viên về Xanh SM: `01-problem-scan.md`, `02-deep-dive-report.md`, `03-ai-log.md`, `04-workflow-diagram.png`, `starter-code/prompt_prototype.py`. File `05-validation.md` ghi kiểm tra; file này hướng dẫn nộp. Chưa commit hoặc push.

Remote origin hiện trỏ tới **GDGoC-FPTU/VinUni_Codelab_Day02_Template**, là repo mẫu. Không dùng remote này để nộp trừ khi giảng viên xác nhận. Chưa có link repo nhóm/tên nhánh, nên các lệnh dưới có chỗ cần thay.

## 2. Việc bạn cần hoàn tất

1. Điền họ tên/MSSV trong scan và AI log; đọc hiểu, sửa phản ánh bằng lời của mình.
2. Xác nhận chủ đề với nhóm; các số liệu đang là giả định, không ghi thành dữ kiện thực tế.
3. Trưởng nhóm tạo repo từ template hoặc nhận repo Classroom, thêm 4–6 thành viên với quyền write; bạn nhận lời mời.
4. Chạy Gemini thật và bổ sung kết quả trước khi tuyên bố prototype thành công.

## 3. Kiểm thử trên PowerShell

```powershell
python -m venv .venv
.venv\Scripts\python.exe -m pip install -r requirements.txt
.venv\Scripts\python.exe starter-code/prompt_prototype.py --offline
```

Nhập key ngay trong terminal máy bạn, không dán vào code hoặc chat. Đoạn sau tránh hiển thị key khi nhập:

```powershell
$geminiSecret = Read-Host 'Gemini API key' -AsSecureString
$env:GEMINI_API_KEY = [System.Net.NetworkCredential]::new('', $geminiSecret).Password
.venv\Scripts\python.exe starter-code/prompt_prototype.py
.venv\Scripts\python.exe autograder/autograder.py
Remove-Item Env:GEMINI_API_KEY
```

Script dùng model `gemini-2.5-flash` theo đề. Nếu tài khoản không truy cập được model, hỏi giảng viên trước khi đổi model; không nhận fixtures offline là kết quả live. Các kiểm tra tự động không thay thế việc đọc từng phản hồi để phát hiện bịa dữ kiện hoặc chỉ dẫn sai.

## 4. Đưa bài cá nhân lên repo nhóm

Thực hiện trong thư mục hiện tại, thay URL và tên nhánh bằng thông tin thật. Thêm remote riêng để giữ nguyên origin của template:

```powershell
git remote add nhom https://github.com/TEN-NHOM/TEN-REPO.git
git fetch nhom
git switch -c ten-mssv
git add 01-problem-scan.md 02-deep-dive-report.md 03-ai-log.md 04-workflow-diagram.png starter-code/prompt_prototype.py 05-validation.md 06-huong-dan-nop-nhom.md
git commit -m "Complete individual Lab 02: ten-mssv"
git push -u nhom ten-mssv
```

Nếu remote `nhom` hoặc nhánh đã tồn tại, kiểm tra `git remote -v` và `git branch`, dùng tên hiện có thay vì tạo lại. Nếu repo nhóm có lịch sử khác template, hãy clone repo nhóm sang thư mục mới, tạo nhánh cá nhân từ main của repo nhóm rồi copy đúng các file bài làm sang đó. Không force push hoặc gộp lịch sử không liên quan.

Gửi trưởng nhóm link `https://github.com/TEN-NHOM/TEN-REPO/tree/ten-mssv`. Không gửi pull request gộp toàn bộ nhánh chứa thay đổi `.py` vào main.

## 5. Trưởng nhóm tổng hợp sau khi tất cả thành viên push

Nhóm review các bài, chọn/chỉnh sửa báo cáo và ghi đóng góp thực tế của từng người. Trưởng nhóm thực hiện trong clone sạch của repo chung, nơi `origin` trỏ repo nhóm:

```powershell
git fetch origin
git switch main
git pull --ff-only origin main
git switch -c tong-hop-bao-cao
git restore --source origin/ten-mssv -- 01-problem-scan.md 02-deep-dive-report.md 03-ai-log.md 04-workflow-diagram.png
# Chinh sua bao cao theo ket qua review cua ca nhom
git add 01-problem-scan.md 02-deep-dive-report.md 03-ai-log.md 04-workflow-diagram.png
git diff --cached --name-only
git commit -m "Select and consolidate group Lab 02 reports"
git push -u origin tong-hop-bao-cao
```

Tạo PR `tong-hop-bao-cao` → `main`, kiểm tra tab Files changed chỉ có tài liệu/sơ đồ được chọn rồi merge. Không lấy thay đổi code `.py` cá nhân vào main. File starter `.py` có sẵn từ template không đồng nghĩa phải xóa nó; điều cần tránh là gộp lời giải code cá nhân vào main.

## 6. Nộp chính thức

- Main có 3 báo cáo và sơ đồ cuối cùng; nội dung thống nhất, ghi đúng giả định và kết quả thật.
- Mỗi thành viên có nhánh riêng với code, báo cáo, commit của mình; giữ nhánh để giảng viên chấm.
- Kiểm tra quyền truy cập repo theo hướng dẫn giảng viên/Classroom.
- **Chỉ trưởng nhóm** điền họ tên trưởng nhóm và link repository chung vào form/LMS giảng viên cung cấp. Thành viên khác không điền form trùng.
- Đề trong thư mục không có URL form hoặc hạn nộp; lấy hai thông tin này từ lớp, không tự đoán.
