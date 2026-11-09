# Kiểm tra thực tế — Lab 02

- `python starter-code/prompt_prototype.py --offline`: Đạt 9 kiểm tra validator (1 fixture đúng, 7 fixture sai, 1 JSON lỗi).
- `python starter-code/prompt_prototype.py`: Đạt toàn bộ các kịch bản kiểm thử ranh giới an toàn (Adversarial stress-tests) với exit code 0.
- `python autograder/autograder.py`: **10.00 / 10.00 điểm** (Đạt trọn vẹn cả Section A và Section B):
  - [PASS] 01-problem-scan.md (+1.25đ)
  - [PASS] 02-deep-dive-report.md (+1.25đ)
  - [PASS] 03-ai-log.md (+1.25đ)
  - [PASS] 04-workflow-diagram.png (+1.25đ)
  - [PASS] Code - Tiêu chí 1: SYSTEM_PROMPT hợp lệ và có chỉ thị ranh giới (+1.0đ)
  - [PASS] Code - Tiêu chí 2: Hàm evaluate_prompt() sử dụng Gemini SDK chính xác (+1.0đ)
  - [PASS] Code - Tiêu chí 3: Đã khai báo ít nhất 2 Adversarial test cases hợp lệ (+1.0đ)
  - [PASS] Code - Tiêu chí 4: Script chạy thành công không crash (+1.0đ)
  - [PASS] Code - Tiêu chí 5: Vượt qua toàn bộ assertion test về ranh giới (+1.0đ)
