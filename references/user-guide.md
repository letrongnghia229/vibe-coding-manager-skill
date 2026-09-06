# Vibe Coding Manager — Hướng dẫn cài đặt và sử dụng

## Mục lục

1. Skill dùng để làm gì
2. Cài vibe-coding-manager
3. Có cản cài lên Codex không
4. Kết hợp ui-ux-pro-max
5. Cài ui-ux-pro-max cho Codex
6. Setup project
7. Cách sử dụng hằng ngày
8. UI workflow
9. Khi nào dùng ui-ux-pro-max
10. Model selection
11. GitHub trong workflow
12. Final Phase flow
13. Quy tắc cần nhớ
14. Quick-start


## 1. Skill này dùng để làm gì?

`vibe-coding-manager` giúp quản lý toàn bộ workflow phát triển phần mềm bằng ChatGPT Web + Codex:

- brainstorm ý tưởng;
- phản biện proposal;
- chốt spec;
- chia Phase / Round;
- chọn model cho Codex;
- tạo `/plan`;
- review `/plan`;
- tạo `/goal`;
- review implementation;
- hướng dẫn manual QA;
- UI Design Gate + Visual QA;
- diagnose / Fix Round;
- Final Regression Review;
- Final Verification / Delta Verification;
- checkpoint Git.

Skill này không thay Codex. ChatGPT dùng skill để điều phối; Codex thực thi code.

## 1.1. Mặc định cho người không có nền tảng công nghệ

Nếu bạn nói rằng mình không có nền tảng lập trình, skill sẽ giữ ba mặc định xuyên suốt project:

1. **Ngôn ngữ theo bạn** — ChatGPT trả lời bằng ngôn ngữ bạn đang dùng; prompt gửi Codex cũng dùng ngôn ngữ đó và yêu cầu Codex phản hồi cùng ngôn ngữ.
2. **Giải thích trước, kỹ thuật sau** — khi có quyết định kiến trúc, skill giải thích nó ảnh hưởng gì đến sản phẩm trước khi nói về database, lock, migration, state machine...
3. **Chống overengineering** — trước khi chấp nhận kiến trúc phức tạp, skill phải so sánh với giải pháp tối thiểu và hỏi: nếu dùng cách đơn giản thì điều tệ nhất thực tế là gì?

Nếu giải pháp phức tạp chỉ để tránh một edge case hiếm, có thể phục hồi và không làm mất dữ liệu/bảo mật, skill sẽ ưu tiên giải pháp đơn giản hơn hoặc dừng để bạn duyệt trade-off trước.

Bạn không cần nhắc lại những điều này ở mỗi Round nếu chúng đã có trong Project context.

## 2. Cài `vibe-coding-manager` ở đâu?

### Khuyến nghị

Cài skill này vào **ChatGPT Web / Skills**.

Lý do:
- ChatGPT là nơi quản lý product decision, review, Phase/Round và acceptance gate;
- Codex là nơi làm việc trực tiếp với repository;
- nếu cài manager skill lên Codex, vai trò dễ bị chồng chéo.

### Cách cài

1. Tải file `skill.zip` được cung cấp.
2. Mở khu vực Skills của ChatGPT (`/skills` nếu workspace của bạn hỗ trợ đường dẫn này).
3. Upload/import `skill.zip` theo giao diện Skills hiện có.
4. Kiểm tra skill `Vibe Coding Manager` xuất hiện trong thư viện.
5. Dùng trong ChatGPT Project chứa dự án của bạn.

Nếu workspace không cho upload skill, quyền Skills có thể bị giới hạn bởi plan/workspace policy.

## 3. Có cần cài `vibe-coding-manager` lên Codex không?

**Không cản và mặc định không nên.**

Mô hình khuyến nghị:

```text
ChatGPT Web + vibe-coding-manager
       ↓
quyết định / review / prompt
       ↓
Codex
       ↓
repository / code / tests / git
```

Chỉ tạo một biến thể Codex-specific trong tương lai nếu bạn quyết định dùng Codex làm orchestrator chính.

## 4. Kết hợp với `ui-ux-pro-max`

Upstream:
https://github.com/nextlevelbuilder/ui-ux-pro-max-skill

### Có bắt buộc không?

Không.

Bạn có hai mode:

#### Mode A — đơn giản nhất

- `vibe-coding-manager` ở ChatGPT Web;
- ChatGPT chốt UI direction / Master Design System;
- lưu `MASTER.md` hoặc UI spec vào project;
- Codex triển khai theo spec;
- không cần cài ui-ux-pro-max lên Codex.

Phù hợp khi UI đơn giản hoặc Master đã rõ.

#### Mode B — khuyến nghị cho project UI-heavy

- `vibe-coding-manager` ở ChatGPT Web;
- `ui-ux-pro-max` cài trong project Codex;
- Codex dùng specialist này trong UI `/plan` và `/goal`;
- `MASTER.md` / UI spec đã được user duyệt vẫn là source of truth.

Phù hợp với web app, dashboard, desktop-like UI, hoặc app cần modernization.

## 5. Cài `ui-ux-pro-max` cho Codex

Theo upstream project:

```bash
npm install -g ui-ux-pro-max-cli
cd /path/to/your/project
uipro init --ai codex
```

Python 3.x cản có để search script hoạt động.

Không cản cài cho tất cả project. Cài theo project khi project đó có UI đáng kể là cách dễ kiểm soát nhất.

### Update

Dùng CLI upstream để update thay vì copy thủ công. Kiểm tra README của upstream khi version thay đổi.

## 6. Setup project khuyến nghị

Trong ChatGPT Project:

```text
Project
├─ Project Instructions
├─ PRD / Roadmap / Architecture docs
├─ UI/UX spec
├─ Checkpoint docs
├─ GitHub repo connection (nếu dùng)
├─ Chat: 00 Project Control
├─ Chat: Phase N
└─ Chat: Independent Review Phase N
```

Trong repository:

```text
docs/
├─ PRD...
├─ ROADMAP...
├─ CODEX_RULES...
└─ CHECKPOINTS...

design-system/
└─ <project>/
   ├─ MASTER.md
   └─ pages/
      └─ <page>.md
```

Không bắt buộc tên file giống hệt; quan trọng là source of truth rõ ràng.

## 7. Cách nói chuyện với skill hằng ngày

Bạn không cản dùng prompt dài.

### Brainstorm

```text
Tôi muốn thêm bookmark vào video.
Brainstorm theo workflow vibe coding của tôi.
Chưa code.
```

### Phản biện

```text
Đây là proposal đã có.
Phản biện trước khi chốt.
```

### Chia Round

```text
Requirement đã chốt.
Chia Phase này thành các Round nhỏ giúp tôi.
```

### Tạo `/plan`

```text
Round 2 đã chốt.
Cho tôi prompt Codex /plan.
```

### Review `/plan`

```text
Đây là /plan Codex trả về:
<paste>

Review giúp tôi trước khi /goal.
```

### Tạo `/goal`

```text
Plan PASS.
Cho tôi prompt /goal.
```

Skill phải tự merge các guardrail phát hiện trong lúc review vào `/goal`.

### Sau implementation

```text
Codex báo xong như sau:
<paste>

Tôi chưa manual test.
Cho tôi checklist.
```

### Bug / screenshot

```text
Manual test bước 4 bị lỗi.
Expected: ...
Actual: ...
Ảnh/log đính kèm.
Theo workflow, giờ nên làm gì?
```

Skill tự quyết định diagnose-first hay tạo Fix Round.

### Round PASS

```text
Round này tôi test OK hết.
```

Skill tự xác định còn Round hay đang ở cuối Phase.

### Final Regression

```text
Tất cả Round của Phase đã PASS.
Chuẩn bị Final Regression Review trước checkpoint.
```

### Checkpoint

```text
Final manual smoke PASS hết.
Cho tôi prompt checkpoint.
```

## 8. UI workflow

### Feature có UI mới

```text
Functional spec
→ UI Design Gate
→ UI specialist / proposal
→ user chọn direction
→ freeze UI spec
→ Round planning
→ Codex /plan
→ /goal
→ Functional QA
→ Visual QA
```

### App đã chạy nhưng UI xấu

Không rewrite một lần.

```text
UI Audit
→ KEEP / POLISH / REDESIGN / INCONSISTENT
→ 2–3 design directions
→ user chọn
→ MASTER.md
→ tokens/shared components
→ migrate từng screen/flow theo Round
→ Visual QA sau mới Round
```

## 9. Khi nào dùng `ui-ux-pro-max` trong quy trình?

### Dùng ở UI Design Gate

Để:
- tạo candidate design system;
- chọn style/layout/color/typography;
- kiểm accessibility/responsive;
- tạo page-specific guidance.

### Dùng trong Codex `/plan`

Khi skill đã cài ở project Codex, thêm yêu cầu:

```text
If ui-ux-pro-max is installed, use it for UI design intelligence and stack-specific guidance.
The approved MASTER.md / page UI spec is authoritative.
Do not regenerate or replace the Master.
```

### Dùng trong `/goal`

Chỉ để triển khai đúng approved Master/UI spec, không để mở scope.

### Dùng ở Visual QA

Dùng guideline của specialist để review screenshot, nhưng final acceptance vẫn do user.

## 10. Model selection

Skill chọn động theo risk của Round:

- **Terra Extra High**: mặc định cho feature / bug / UI implementation.
- **Sol High/Extra High**: architecture, data/migration, concurrency, destructive operations, root cause khó, Final Regression Review.
- **Luna**: task thật sự nhỏ và cô lập.

Giữ cùng model trong một Codex Round session từ `/plan` đến implementation/manual QA handoff.

## 11. GitHub trong workflow

GitHub là stable remote source of truth, không phải lúc nào cũng là current working tree.

Ví dụ:

```text
GitHub: v0.8 stable
Codex local: v0.8 + Round 1 + Round 2 chưa commit
```

ChatGPT đọc GitHub chỉ thấy stable code. Khi review current Round, dùng Codex report/diff/screenshot/log.

Không push mỗi Round chỉ để ChatGPT đọc, trừ khi project cố ý dùng branch-based review.

## 12. Final Phase flow

```text
All Rounds PASS
→ new Sol review session
→ Final Regression Review
→ blocker? Fix Round
→ Final Verification
→ Final Manual Smoke
→ post-review fix? Delta Verification hoặc full re-review
→ user final PASS
→ checkpoint
→ commit + tag + push + verify remote
→ Next Phase
```

## 13. Quy tắc cản nhớ

1. Một Round, một mục tiêu chính.
2. Một Round, một Codex session, một model.
3. `/plan` trước `/goal` cho task đáng kể.
4. Review `/plan` trước khi code.
5. `/goal` phải chứa guardrail từ plan review.
6. Prompt Codex và report Codex dùng cùng ngôn ngữ với user, trừ khi user yêu cầu khác.
7. Với user không chuyên, skill phải giải thích bằng ngôn ngữ đơn giản và đưa ra một khuyến nghị rõ ràng.
8. Trước khi thêm state machine/scheduler/migration/reconciliation lớn, chạy Product Simplicity Review.
9. Edge case hiếm, phục hồi được, không phá dữ liệu không tự động biện minh cho kiến trúc phức tạp.
10. Automated PASS không thay manual PASS.
11. UI Round cần Visual QA.
12. Không workaround để biến missing requirement thành PASS.
13. Không checkpoint khi còn blocker.
14. Stable > Clever.
15. Không refactor stable code chỉ vì đẹp kiến trúc.
16. Không bắt đầu Phase tiếp theo trước stable checkpoint.

## 14. Quick-start cho một project mới

```text
1. Tạo ChatGPT Project.
2. Thêm project instructions và docs.
3. Nếu bạn không có nền tảng công nghệ, ghi rõ `technical level: non-technical`, ngôn ngữ mong muốn và `Stable > Clever`.
4. Cài vibe-coding-manager vào ChatGPT Web.
5. Nếu UI-heavy, cài ui-ux-pro-max vào repo Codex.
6. Brainstorm / critique / freeze spec.
7. Nếu feature bắt đầu phình kiến trúc, chạy Product Simplicity Review trước khi tiếp tục.
8. Nếu có UI đáng kể, chốt Master/UI spec.
9. Chia Phase / Round.
10. Mỗi Round: model → session mới → /plan → review → /goal → test → user QA.
11. Cuối Phase: Sol Final Regression → final verify → manual smoke → checkpoint.
```
