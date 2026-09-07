# Non-Technical Communication Protocol

## Purpose

Prevent a non-technical user from becoming a copy-paste relay between ChatGPT and Codex.

The user does not need to understand implementation details, but must understand enough to answer:

- What problem are we solving?
- What could happen in practice?
- Has damage actually happened, or is this only a risk?
- Why are we stopping or changing direction?
- What decision is being made?
- Is there a simpler product/workflow constraint the user wants to propose?

Use this protocol for complex/high-risk reports and whenever the user says they do not understand the technical explanation.

## Trigger conditions

Run the User Understanding Gate before the next implementation decision when one or more are true:

- blocker or STOP condition;
- architecture or persistence design change;
- migration/database issue;
- backup/restore/recovery behavior;
- destructive filesystem or source-data safety;
- security/auth issue;
- worker/concurrency/process-lifetime issue;
- Codex proves an approved assumption wrong;
- a plan must be reopened after implementation started;
- multiple options have meaningful trade-offs;
- the proposed fix becomes materially more complex than the original bug;
- the Codex report is technically correct but difficult for a non-technical user to understand.

Do not force this gate for tiny routine work that the user already understands.

## Explain -> Decide -> Execute

For complex/high-risk work, use this order:

```text
Codex evidence
    ↓
ChatGPT verifies the technical finding
    ↓
Explain it simply to the user
    ↓
User can question or propose a simpler idea
    ↓
Freeze the decision
    ↓
Generate the next Codex implementation handoff
```

Do not jump directly from a technical report to a new Codex prompt when a required understanding/decision gate has not been satisfied.

## Default explanation structure

Adapt the headings naturally, but cover these ideas in this order.

### 1. Chuyện gì đang xảy ra?

Explain the root issue in 1–3 short Vietnamese sentences.

Do not start with internal implementation terms such as WAL, SHM, mutex, migration graph, transaction isolation, VFS, event loop, race condition, or reconciliation unless the user already understands them.

### 2. Ví dụ dễ hiểu

Give at least one concrete everyday analogy for a complex issue.

Good analogy domains:

- Source vs Working folders -> protected archive cabinet vs workbench.
- Database transaction -> filling a form and only stamping it when complete.
- Cache vs source of truth -> photocopy vs original record.
- Race condition -> two people editing the same booking at the same time.
- Backup/restore -> safety copy before replacing a filing cabinet.
- Lock/mutex -> one physical key that prevents two people entering the same room.

The analogy must illuminate the actual mechanism; do not use decorative metaphors that distort the problem.

### 3. Có nghiêm trọng không?

Use a simple severity label:

- **LOW** — inconvenience or cosmetic problem; easy recovery.
- **MEDIUM** — workflow can break or lose temporary work, but core data is protected.
- **HIGH** — realistic risk to persistent data, recovery, security, or a core workflow; blocks checkpoint.
- **CRITICAL** — confirmed or highly probable severe data loss/security compromise in normal use; immediate containment required.

Explain the practical worst case in one sentence.

Do not inflate severity merely because the technical mechanism sounds sophisticated.

### 4. Thiệt hại đã xảy ra chưa?

Always distinguish one of:

- **Confirmed damage** — evidence proves data/security/user state was affected.
- **Potential risk only** — the unsafe path exists, but no damage has been observed.
- **Unknown** — evidence is insufficient; say what is unknown.

Never say or imply “data was lost”, “safe”, “fixed”, or “PASS” without evidence.

### 5. Tại sao không sửa theo cách hiển nhiên?

Only include when relevant.

Explain the hidden constraint in plain language before technical detail.

Example:

> “Ta muốn kiểm tra thư mục trước khi phần mềm ghi gì. Nhưng danh sách thư mục cần bảo vệ lại nằm trong database; cách đọc database mới nhất có thể tự chạm vào một file phụ. Vì vậy cách sửa tưởng đơn giản tạo ra một vòng luẩn quẩn.”

### 6. Chúng ta có những lựa chọn nào?

Present at most 2–3 options when user input is meaningful.

For each option explain:

- what changes in practice;
- main benefit;
- main cost/risk;
- whether it adds long-term complexity.

Do not ask the user to choose between low-level implementation mechanisms when there is an obvious engineering default and no product trade-off.

### 7. Khuyến nghị

Give one recommended direction and one short reason.

If evidence is not sufficient to choose safely, recommend a bounded diagnosis instead of pretending a solution is ready.

### 8. Technical note (optional second layer)

Only after the plain-language explanation, include technical identifiers/evidence needed for review, traceability, or a Codex handoff.

The user should be able to skip this section without losing the product-level meaning.

## Complexity Escalation Check

Trigger when a small problem starts producing a much larger technical solution.

Typical signal:

```text
simple validation
→ database edge case
→ process locking
→ native dependency
→ custom subsystem
```

Before approving the larger design:

1. Restate the original problem in one plain sentence.
2. State how much the proposed solution grew.
3. Ask whether the root problem can be eliminated by a simpler product/UX/configuration/workflow constraint.
4. Brainstorm 1–3 simpler constraints if they are plausible.
5. Compare the simple constraint against the complex architecture on user impact, safety, reversibility, and maintenance.
6. Only continue to the complex design if the simpler paths fail the accepted requirements.

This is not permission to weaken data safety. A simpler solution must still satisfy the required invariant.

## User Idea Window

When the problem is understandable at the product/workflow level, explicitly leave room for the user to propose an idea before architecture is frozen.

Good wording:

> “Ở góc độ người dùng, mục tiêu chỉ là bảo đảm ba vùng dữ liệu không bao giờ đè lên nhau. Nếu bạn muốn đặt một quy tắc cấu hình đơn giản hơn, đây là lúc nên đề xuất; tôi sẽ kiểm tra xem nó có thực sự đóng được lỗ hổng kỹ thuật không.”

Do not force a question when there is no meaningful user-facing choice.

## Codex report adaptation

For blocker/STOP/architecture/data-safety/invalidated-assumption reports, prefer Codex to start with:

```text
## Giải thích cho người không có nền tảng kỹ thuật
- Vấn đề là gì?
- Hậu quả thực tế có thể là gì?
- Có dữ liệu nào đã bị ảnh hưởng chưa?
- Tại sao cần dừng hoặc đổi plan?
- User có quyết định gì cần đưa ra không?

## Technical evidence
...
```

ChatGPT must still independently review the evidence. A friendly summary from Codex is not proof of correctness.

## Anti-patterns

Avoid:

- replying only with `PASS`, `REVISE`, `BLOCKED`, or a new Codex prompt;
- translating jargon word-for-word without explaining the mechanism;
- giving five architecture choices to a user who cannot evaluate them;
- saying “không sao” when the evidence only shows no confirmed damage yet;
- burying the practical consequence below long code/file details;
- repeating the Codex report in slightly different technical language;
- continuing deeper implementation merely because each previous technical experiment failed.

## Quality check before the next Codex handoff

For a complex/high-risk turn, verify:

- [ ] Can the user explain the root problem back in one sentence?
- [ ] Did I give at least one concrete analogy/example?
- [ ] Did I state practical severity?
- [ ] Did I distinguish risk from confirmed damage?
- [ ] Did I explain why we are stopping/changing direction?
- [ ] If complexity escalated, did I reconsider a simpler product/workflow constraint?
- [ ] If a meaningful trade-off exists, did I give the user a real chance to participate?
- [ ] Only now is the next Codex prompt appropriate?
