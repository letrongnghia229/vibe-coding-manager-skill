# Vibe Coding Manager — Hướng dẫn cài đặt và sử dụng

## Mục lục

1. Skill dùng để làm gì
2. Cài vibe-coding-manager
3. Có cần cài lên Codex không
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
14. Tiết kiệm Codex usage
15. Cấu trúc repository
16. Quick-start


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

**Không cần và mặc định không nên.**

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

Python 3.x cần có để search script hoạt động.

Không cần cài cho tất cả project. Cài theo project khi project đó có UI đáng kể là cách dễ kiểm soát nhất.

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

Trong repository, ưu tiên cấu trúc đang có và chỉ thêm những phần thực sự cần. Một mẫu đơn giản:

```text
AGENTS.md
src/ hoặc backend/frontend/
tests/
docs/
├─ PRD...
├─ ROADMAP...
├─ CODEX_RULES...
└─ CHECKPOINTS...
```

`AGENTS.md` phải ngắn: nó là bản đồ/routing cho Codex, không phải bản sao PRD. Nếu project có design system đã duyệt thì có thể thêm `design-system/`. Không restructure repo chỉ để trông “chuẩn”.

## 7. Cách nói chuyện với skill hằng ngày

Bạn không cần dùng prompt dài.

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
→ Visual QA sau mỗi Round
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

Skill chọn **model + reasoning effort rẻ nhất nhưng vẫn an toàn** theo risk của cả Round:

- **Luna**: task rất nhỏ, cơ học, cô lập, dễ verify, không persistence/migration/security/destructive behavior.
- **Terra Medium/High**: phần lớn feature, UI, API và bug có phạm vi rõ. Medium khi task rõ; High khi cần reasoning nhiều hơn.
- **Sol High**: migration, data safety, recovery/restore, worker/concurrency, security, destructive operations, root cause khó, high-risk/final review.
- **Sol Extra High**: ngoại lệ khi failure cost/uncertainty đặc biệt cao hoặc Sol High chưa xử lý đáng tin cậy.

Không dùng Extra High chỉ để “cho chắc”. Giữ cùng model trong một Codex Round session; nếu `/plan` làm lộ risk cao hơn đáng kể thì dừng và mở session mới với model phù hợp.

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

## 13. Quy tắc cần nhớ

1. Một Round, một mục tiêu chính.
2. Một Round, một Codex session, một model.
3. `/plan` trước `/goal` cho task đáng kể.
4. Review `/plan` trước khi code.
5. `/goal` phải chứa guardrail từ plan review.
6. Automated PASS không thay manual PASS.
7. UI Round cần Visual QA.
8. Không workaround để biến missing requirement thành PASS.
9. Không checkpoint khi còn blocker.
10. Stable > Clever.
11. Không refactor stable code chỉ vì đẹp kiến trúc.
12. Không bắt đầu Phase tiếp theo trước stable checkpoint.

## 14. Tiết kiệm Codex usage

Mặc định dùng mô hình context ba lớp:

```text
AGENTS.md = luật repo ổn định, ngắn
Project docs = chi tiết WHAT/architecture/spec
Prompt = việc đang làm NOW + evidence/delta
```

Quy tắc chính:

1. Một Round = một Codex session = một model.
2. `/plan` compact; chỉ dẫn Codex đọc `AGENTS.md` và docs/file liên quan.
3. Sau `/plan` PASS, `/goal` chỉ nói “implement approved plan from this session” + review delta; không paste lại plan.
4. Plan cần sửa thì ưu tiên correction delta, không viết lại toàn bộ nếu không cần.
5. Focused tests trước; full suite ở risk/gate phù hợp.
6. Nếu automated verification + review + manual QA đã PASS và code không đổi, không chạy lại cùng verification chỉ để lặp evidence.
7. Independent review chỉ dùng khi risk xứng đáng.
8. Báo cáo Codex ngắn; không paste full diff/log khi không có lỗi cần xem.
9. Giữ CWD/model/tool set ổn định trong Round khi thực tế cho phép để hỗ trợ prompt caching tự động.
10. Không dùng multi-agent/Best-of-N mặc định.

Chi tiết: `references/token-efficiency.md`.

## 15. Cấu trúc repository

Skill không ép một cây thư mục cứng cho mọi project. Nguyên tắc là:

- giữ cấu trúc hiện có trước;
- top-level ít và dễ hiểu;
- mỗi concern có một nơi chính;
- không tạo folder theo Phase/Round;
- runtime/generated data không nằm trong repo trừ fixture/test data có chủ đích;
- tránh nhiều layer/abstraction nếu chưa có nhu cầu thật;
- test phải dễ tìm theo subsystem;
- docs có một canonical source of truth, tránh `final_v2_latest`;
- chỉ restructure khi có vấn đề cụ thể và lợi ích rõ ràng.

Với người không có nền tảng công nghệ, ChatGPT/Codex phải tự đề xuất nơi đặt code phù hợp và chỉ hỏi user khi quyết định ảnh hưởng product behavior, data safety, chi phí hoặc maintainability.

Chi tiết: `references/repository-structure.md`.

## 16. Quick-start cho một project mới

```text
1. Tạo ChatGPT Project.
2. Thêm project instructions và canonical docs.
3. Tạo root `AGENTS.md` ngắn để route Codex tới đúng docs/rules.
4. Cài vibe-coding-manager vào ChatGPT Web.
5. Nếu UI-heavy, cài ui-ux-pro-max vào repo Codex khi thực sự cần.
6. Brainstorm / critique / freeze spec.
7. Nếu có UI đáng kể, chốt Master/UI spec.
8. Chia Phase / Round.
9. Mỗi Round: risk → model/effort → session mới → compact /plan → review → compact /goal → focused verification → user QA.
10. Cuối Phase: independent Final Regression → final verify → manual smoke → checkpoint.
```
