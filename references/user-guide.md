# Vibe Coding Manager — Hướng dẫn cho người không chuyên

## Mục lục

1. Skill này dùng để làm gì
2. Bốn vai trò cần hiểu
3. Workflow hằng ngày
4. `/plan` và `/goal`
5. Prompt mẫu
6. Ngôn ngữ và cách giải thích
7. Chống overengineering
8. Git và checkpoint
9. Đồng bộ skill ở 3 nơi
10. Khi update chính skill
11. Setup ChatGPT Project
12. UI workflow
13. Chọn model Codex
14. Cuối Phase
15. FAQ

## 1. Skill này dùng để làm gì?

`vibe-coding-manager` giúp người không chuyên phát triển phần mềm bằng ChatGPT + Codex mà không cần tự viết code hay tự chọn kiến trúc kỹ thuật chi tiết.

Luồng cơ bản:

```text
Bạn nói mục tiêu
→ ChatGPT + skill phân tích và lập kế hoạch
→ Codex thực hiện code và test
→ bạn kiểm tra kết quả thực tế
→ ChatGPT hướng dẫn bước tiếp theo
```

## 2. Bốn vai trò cần hiểu

### Bạn

Bạn:
- nói sản phẩm cần làm gì;
- cung cấp ảnh/log/ngữ cảnh khi cần;
- test phần mềm thực tế;
- chốt OK cuối cùng.

Bạn không cần tự sửa code.

### ChatGPT + vibe-coding-manager

ChatGPT là nơi bạn làm việc chính. Skill giúp ChatGPT:
- brainstorm và phản biện;
- chia Phase / Round;
- tạo prompt `/plan` và `/goal`;
- review plan và kết quả Codex;
- phát hiện overengineering;
- hướng dẫn manual QA;
- kiểm tra checkpoint.

### Codex

Codex là nơi thực thi:
- đọc repository;
- sửa code;
- chạy test/build;
- báo cáo kết quả.

### GitHub

GitHub lưu:
- phiên bản ổn định;
- lịch sử thay đổi;
- tài liệu và source chuẩn lâu dài.

## 3. Workflow hằng ngày

Đối với một tính năng thông thường:

1. Bạn nói ý tưởng với ChatGPT.
2. ChatGPT làm rõ phạm vi.
3. Chốt requirement.
4. Chia Phase / Round nhỏ nếu cần.
5. ChatGPT tạo prompt `/plan`.
6. Bạn gửi prompt cho Codex.
7. Gửi `/plan` Codex trả về lại ChatGPT.
8. ChatGPT review: `PASS` hoặc `REVISE`.
9. Khi plan PASS, ChatGPT tạo `/goal`.
10. Codex triển khai và test.
11. Gửi báo cáo Codex về ChatGPT.
12. ChatGPT hướng dẫn test tay.
13. Chỉ khi bạn test OK mới coi Round hoàn thành.

Nguyên tắc: **1 Round = 1 mục tiêu chính**.

## 4. `/plan` và `/goal` là gì?

### `/plan`

Dùng để Codex đọc code và đề xuất cách làm trước khi sửa. ChatGPT sẽ review `/plan` để kiểm tra phạm vi, rủi ro và độ phức tạp.

### `/goal`

Chỉ tạo sau khi `/plan` PASS. `/goal` nói rõ:
- làm gì;
- không làm gì;
- test gì bắt buộc;
- khi nào phải dừng.

## 5. Prompt mẫu dùng hằng ngày

### Khi có ý tưởng

```text
Tôi muốn thêm tính năng ...
Hãy brainstorm giúp tôi: mục tiêu, input, output, phạm vi, rủi ro và các câu hỏi còn thiếu.
Chưa code.
```

### Khi requirement đã rõ

```text
Requirement này đã rõ.
Hãy chia thành các Phase / Round nhỏ, mỗi Round có mục tiêu, phạm vi, việc không làm và checklist nghiệm thu.
```

### Khi cần `/plan`

```text
Cho tôi prompt Codex /plan cho Round này.
Viết bằng tiếng Việt và yêu cầu Codex trả lời hoàn toàn bằng tiếng Việt.
```

### Khi Codex trả `/plan`

```text
Đây là /plan Codex trả về.
Hãy review, kiểm tra có overengineering không và kết luận PASS hay REVISE.
```

### Khi plan PASS

```text
Plan đã PASS.
Cho tôi prompt /goal.
Yêu cầu Codex trả lời bằng tiếng Việt.
```

### Khi Codex báo hoàn thành

```text
Đây là báo cáo của Codex.
Tôi chưa test tay.
Hãy giải thích kết quả cho tôi dễ hiểu và cho tôi checklist test.
```

### Khi gặp lỗi

```text
Tôi test bước ... bị lỗi.
Expected: ...
Actual: ...
Tôi gửi kèm ảnh/log.
Hãy giúp tôi xác định bước tiếp theo.
```

## 6. Ngôn ngữ và cách giải thích

Khi bạn đang nói tiếng Việt:
- ChatGPT trả lời bằng tiếng Việt;
- prompt gửi Codex viết bằng tiếng Việt;
- Codex được yêu cầu phản hồi bằng tiếng Việt;
- thuật ngữ kỹ thuật được giải thích đơn giản khi cần.

Tên file, API, hàm hoặc code có thể giữ tiếng Anh để tránh sai kỹ thuật.

## 7. Chống overengineering

Skill phải kiểm tra:
- V1 thực sự cần gì?
- Có cách đơn giản hơn không?
- Nếu dùng cách đơn giản thì điều tệ nhất thực tế là gì?
- Điều đó có gây mất dữ liệu, lộ thông tin, phá dữ liệu hay làm hỏng core workflow không?

Nếu kiến trúc rất phức tạp chỉ để tránh một edge case hiếm, có thể phục hồi và không nguy hiểm, ưu tiên cách đơn giản hơn.

Nguyên tắc: **Stable > Clever**.

## 8. Git và checkpoint

Thông thường:

```text
Codex code
→ test tự động
→ bạn test tay
→ review cuối
→ checkpoint
→ commit / tag / push
```

Không commit/tag chỉ vì Codex báo test PASS. Bạn là acceptance gate cuối cùng.

## 9. Đồng bộ vibe-coding-manager ở 3 nơi

### ChatGPT — nơi bạn yêu cầu thay đổi

Bạn chỉ cần nói muốn thay đổi gì. ChatGPT có thể giúp sửa skill, tài liệu, ảnh hướng dẫn, validate, đóng gói và đồng bộ GitHub khi được yêu cầu.

Bạn không cần tự sửa `SKILL.md`.

### GitHub — nơi lưu phiên bản chuẩn lâu dài

Repo lưu source, references, docs, ảnh hướng dẫn và lịch sử commit.

### Máy tính — bản clone để đọc

Bản clone trên máy chủ yếu dùng để:
- đọc `docs/usage-guide.md`;
- xem ảnh hướng dẫn;
- giữ bản offline.

Bạn không cần chỉnh skill bằng tay ở đây.

Luồng khuyến nghị:

```text
Bạn yêu cầu trong ChatGPT
→ ChatGPT cập nhật skill
→ ChatGPT cập nhật GitHub
→ bạn mở GitHub Desktop và Pull origin
→ máy tính có bản mới để đọc
```

Sau đó dùng `skill.zip` mới để cập nhật Skill trong ChatGPT.

## 10. Khi update chính vibe-coding-manager

Mỗi lần skill thay đổi đáng kể phải kiểm tra đồng thời:
- `SKILL.md`;
- các file trong `references/`;
- `docs/usage-guide.md` nếu repo lưu guide này;
- toàn bộ ảnh hướng dẫn;
- `skill.zip`.

Phân loại ảnh:
- **KEEP** — vẫn đúng;
- **UPDATE** — cần chỉnh nhỏ;
- **REPLACE** — workflow đã đổi;
- **DELETE** — trùng/thừa/gây rối;
- **NEW** — cần ảnh mới.

Không coi update hoàn thành nếu skill đã đổi nhưng guide/ảnh còn mô tả workflow cũ.

## 11. Setup ChatGPT Project

Một project nên có các nguồn quan trọng như:

```text
Project Instructions
PRD / Roadmap
Architecture / Tech Stack
UI/UX Spec
Checkpoint docs
GitHub repository
```

ChatGPT Project giữ ngữ cảnh. Skill cung cấp workflow. Codex thực thi code.

## 12. UI workflow

Nếu thay đổi UI đáng kể:

```text
Requirement
→ chốt hướng UI
→ review
→ user duyệt
→ Codex implement
→ Functional QA
→ Visual QA
```

Không để Codex tự redesign toàn bộ giao diện khi hướng thiết kế chưa được duyệt.

`ui-ux-pro-max` là công cụ hỗ trợ tùy chọn cho project UI-heavy, không phải yêu cầu bắt buộc.

## 13. Chọn model Codex

Skill sẽ đề xuất model theo rủi ro:
- **Terra Extra High**: phần lớn feature / bug / UI thông thường;
- **Sol High/Extra High**: architecture, migration, data safety, concurrency, root cause khó, final review;
- **Luna**: việc nhỏ, rõ, cô lập.

Một Round giữ cùng một model trong cùng Codex session.

## 14. Cuối Phase

Khi tất cả Round đã PASS:

```text
Independent Final Regression Review
→ Final Verification
→ Final Manual Smoke
→ user PASS
→ checkpoint
→ commit + tag + push
```

Không tự động sang Phase mới trước checkpoint ổn định.

## 15. FAQ

### Tôi có cần biết lập trình không?
Không. Bạn cần hiểu sản phẩm mình muốn và kiểm tra kết quả thực tế.

### Tôi có cần tự sửa skill không?
Không. Bạn có thể yêu cầu ChatGPT sửa.

### Tôi có cần tự viết prompt Codex không?
Không. Skill có thể tạo prompt cho bạn.

### Tôi có cần biết Git không?
Không bắt buộc. Với bản clone skill trên máy, thao tác thường xuyên nhất là `Pull origin`.

### Máy tính của tôi có phải nguồn chính của skill không?
Không. Máy tính chủ yếu giữ bản clone để đọc.

### GitHub để làm gì?
Lưu phiên bản chuẩn và lịch sử thay đổi.

### ChatGPT để làm gì?
Đây là nơi bạn điều khiển workflow và yêu cầu thay đổi.

## Quy tắc dễ nhớ

**Bạn nói mục tiêu → ChatGPT quản lý → Codex thực hiện → bạn kiểm tra → GitHub lưu kết quả.**

Khi update chính skill:

**ChatGPT sửa → GitHub lưu → máy tính Pull về để đọc.**
