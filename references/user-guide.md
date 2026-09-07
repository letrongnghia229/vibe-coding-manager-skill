# Vibe Coding Manager — Hướng dẫn sử dụng

## Skill này làm gì?

`vibe-coding-manager` giúp ChatGPT quản lý workflow phát triển phần mềm với Codex:
- brainstorm, critique, freeze spec;
- chia Phase / Round;
- chọn model;
- tạo/review `/plan` và `/goal`;
- diagnose/Fix Round;
- automated/manual/visual QA;
- final regression, verification, checkpoint;
- giải thích blocker/high-risk finding theo cách người không biết lập trình vẫn hiểu.

ChatGPT là manager/reviewer; Codex là executor; user là acceptance/decision gate.

## Cài đặt

Cài `skill.zip` vào ChatGPT Skills. Mặc định không cần cài manager skill vào Codex.

Mô hình khuyến nghị:

```text
ChatGPT + vibe-coding-manager
        -> quyết định / review / giải thích / Codex prompt
Codex   -> inspect repo / code / tests / git khi được phép
GitHub  -> stable remote history / checkpoint source of truth
```

## Workflow hằng ngày

1. Nói mục tiêu/ý tưởng với ChatGPT.
2. Brainstorm/critique và chốt requirement.
3. Chia Phase/Round nếu cần.
4. ChatGPT tạo `/plan`.
5. Gửi `/plan` cho Codex.
6. Mang plan/report Codex về ChatGPT để review.
7. Nếu `PASS`, ChatGPT tạo `/goal` trong cùng Codex session.
8. Codex implement + focused verification.
9. ChatGPT review report.
10. User manual QA; UI Round thêm Visual QA.
11. Chỉ khi acceptance PASS mới `ROUND_ACCEPTED`.
12. Cuối Phase: independent Final Regression -> Final Verification -> Final Manual Smoke -> checkpoint.

Không tự động commit/tag/push trước acceptance và không tự động bắt đầu Phase tiếp theo.

## Prompt ngắn bạn có thể dùng

### Brainstorm

```text
Tôi muốn thêm tính năng ...
Brainstorm theo workflow vibe coding. Chưa code.
```

### Tạo `/plan`

```text
Round này đã chốt. Cho tôi prompt Codex /plan.
```

### Review Codex

```text
Đây là /plan/report Codex trả về:
<paste>
Review giúp tôi trước khi bước tiếp.
```

### Khi không hiểu báo cáo kỹ thuật

```text
Tôi không hiểu báo cáo này.
Giải thích như cho học sinh lớp 10, có ví dụ đời thường,
nói rõ mức độ nghiêm trọng và đã có thiệt hại thật chưa.
Chưa đưa prompt Codex tiếp theo cho đến khi tôi hiểu vấn đề.
```

Với blocker/high-risk finding, skill mới phải tự làm điều này mà bạn không cần nhắc lại.

## User Understanding Gate

Đây là gate mới để tránh workflow kiểu:

```text
Codex report kỹ thuật
-> ChatGPT PASS/REVISE
-> ChatGPT prompt kỹ thuật mới
-> user copy lại Codex nhưng vẫn không hiểu vì sao
```

Với non-technical user, đó là workflow failure.

Gate tự kích hoạt khi có blocker/STOP, architecture, migration/database, backup/restore/recovery, data safety, destructive filesystem, security, concurrency/process lifetime, assumption quan trọng sai, hoặc fix đang phình thành kiến trúc lớn.

ChatGPT phải giải thích theo thứ tự:

1. **Chuyện gì đang xảy ra?** — 1–3 câu đơn giản.
2. **Ví dụ dễ hiểu** — ít nhất một ví dụ/so sánh đời thường nếu cơ chế phức tạp.
3. **Có nghiêm trọng không?** — LOW / MEDIUM / HIGH / CRITICAL và worst case thực tế.
4. **Thiệt hại đã xảy ra chưa?** — confirmed damage / potential risk / unknown.
5. **Tại sao cách sửa hiển nhiên chưa đủ?** — nếu có hidden constraint.
6. **Có lựa chọn gì?** — tối đa 2–3 lựa chọn khi user input thực sự có ý nghĩa.
7. **Khuyến nghị** — một hướng được đề xuất rõ ràng.
8. **User Idea Window** — khi có thể giải ở tầng product/workflow/configuration, user được đề xuất ý tưởng trước khi architecture bị khóa.

Nguyên tắc mới:

> **Explain -> Decide -> Execute**

Technical detail có thể nằm phía sau; user có thể bỏ qua phần đó mà vẫn hiểu quyết định.

## Ví dụ cách giải thích đúng

Giả sử Source Root chứa dữ liệu gốc và Working Root là nơi app được phép move/delete/cache.

Thay vì nói ngay:

> `topology validation` fail vì `mode=ro` mutate `-shm`...

ChatGPT nên nói trước:

> Source Root giống tủ hồ sơ gốc không được đụng vào; Working Root giống bàn sửa chữa được phép cắt/dán/xóa. Nếu bàn sửa chữa nằm trong tủ hồ sơ, restore có nguy cơ tác động nhầm dữ liệu gốc. Đây là HIGH risk nhưng không có nghĩa dữ liệu đã bị mất; phải tách potential risk khỏi confirmed damage.

Sau đó mới giải thích technical evidence nếu cần.

## Complexity Escalation Check

Khi một fix nhỏ bắt đầu thành:

```text
simple validation
-> database edge case
-> process lock
-> native dependency
-> custom subsystem
```

ChatGPT phải dừng trước khi tiếp tục và kiểm tra lại:
- vấn đề gốc là gì;
- complexity tăng vì requirement thật hay vì implementation choice;
- có product/UX/configuration/workflow constraint đơn giản hơn không;
- cách đơn giản có vẫn giữ data safety/compatibility/user workflow không;
- nếu vẫn cần kiến trúc phức tạp, maintenance/dependency cost là gì.

Đây không phải lý do hạ tiêu chuẩn an toàn. `Stable > Clever`.

## Codex cũng phải giải thích dễ hiểu khi gặp blocker

Các high-risk prompt do skill tạo sẽ yêu cầu Codex prepend:

```text
## Giải thích cho người không có nền tảng kỹ thuật
- Vấn đề là gì?
- Ví dụ đời thường nào giúp hiểu cơ chế này?
- Worst case thực tế là gì?
- Đã có damage thật chưa hay chỉ là risk?
- Vì sao phải STOP/đổi plan/Fix Round?
- User có decision product/workflow thật sự cần đưa ra không?

## Technical evidence
...
```

ChatGPT vẫn phải tự review technical evidence; phần dễ hiểu không thay thế correctness review.

## Current Phase / session handoff

Với project nhiều Round, dùng `docs/CURRENT_PHASE.md` nếu repo áp dụng convention này. File chỉ giữ:
- Phase/Round ledger;
- current state;
- previous stable checkpoint;
- uncommitted/checkpoint status;
- selected model;
- đúng một next action.

Mặc định một ChatGPT conversation quản lý một active Round. Mỗi Round dùng một Codex session/model. Khi mở chat/session mới, skill cung cấp compact Session Handoff thay vì bắt user nhớ transcript.

Không được nói ChatGPT đã đọc local `CURRENT_PHASE.md` nếu environment hiện tại không thật sự có quyền đọc file đó.

## Model selection

- **Luna** — tiny/mechanical/isolated/easy verification.
- **Terra Medium/High** — phần lớn feature/UI/API/bounded bug fix.
- **Sol High** — architecture, migration, data safety, recovery/restore, concurrency, destructive operations, difficult diagnosis, independent high-risk review.
- **Sol Extra High** — chỉ khi failure cost/uncertainty đặc biệt cao hoặc Sol High chưa giải quyết đáng tin cậy.

Giữ một model cho cả Round session trừ khi chủ động abandon/restart vì risk class thay đổi.

## UI workflow

Với UI đáng kể:

`functional requirement -> UI Design Gate -> user approval -> freeze UI spec -> Round -> Functional QA -> Visual QA`

`ui-ux-pro-max` là specialist tùy chọn, không thay workflow manager và không được override approved Master/UI spec.

## GitHub

GitHub thường phản ánh stable/pushed state; current local Codex working tree có thể mới hơn.

Khi review uncommitted Round/Phase, dùng fresh Codex/local diff/report/screenshots/logs. Cuối checkpoint phải verify remote commit/tag và clean local working tree.

## Token/credit efficiency

Ưu tiên:
- `AGENTS.md` ngắn cho stable repo rules;
- `CURRENT_PHASE.md` ngắn cho current state;
- current prompt chỉ mang NOW/delta/evidence;
- `/goal` không lặp `/plan`;
- targeted inspection/focused tests trước broad work;
- compact reports;
- high-risk tăng review/verification depth, không tự động tăng documentation breadth.

Một giải thích ngắn cho user ở đúng tại điểm thường tiết kiệm hưn nhiều vòng copy-paste và diagnosis do user không hieểu quyết định.

## Quy tắc dễ nhớ

1. One Round = one major goal = one Codex session/model.
2. `/plan` trước `/goal` cho work đáng kể.
3. Review plan trước implementation.
4. Automated PASS không thay manual PASS.
5. UI Round cần Visual QA.
6. Không checkpoint khi còn blocker.
7. Không commit/tag/push quá sớm.
8. Không bắt đầu Phase tiếp theo tự động.
9. Potential risk != confirmed damage.
10. Complex/high-risk: **Explain -> Decide -> Execute**.
11. Không để user thành copy-paste relay giữa ChatGPT và Codex.
12. Khi fix phình lớn: chạy Complexity Escalation Check trước khi thêm architecture.
