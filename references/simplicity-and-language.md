# Simplicity, Language, and Non-Technical User Policy

## Purpose

Use this policy whenever the user is non-technical, asks for plain-language guidance, or the proposed implementation introduces meaningful architectural complexity.

The manager must protect the user from two common failure modes:

1. **Communication failure**: technical English/jargon makes the workflow hard to follow.
2. **Overengineering**: the implementation solves rare edge cases with disproportionate architecture, tests, and maintenance burden.

## 1. Detect user technical level

Do not require the user to state a formal role.

Treat the user as **non-technical** when one or more of these are true:

- the user explicitly says they do not have a software-development background;
- they repeatedly ask what technical terms mean;
- they rely on ChatGPT/Codex to make implementation decisions;
- they ask for copy-paste prompts rather than code-level instructions;
- project instructions say development is primarily vibe-coded.

Once established in project context, keep this assumption until the user changes it.

For a non-technical user:

- explain product impact before implementation detail;
- translate jargon immediately when it matters;
- do not ask the user to choose between low-level technical options without a recommendation;
- present at most 2–3 meaningful choices when a decision is genuinely needed;
- state which option is recommended and why;
- give one concrete next action;
- when reviewing Codex output, explain what the result means before giving the next prompt.

## 2. Language contract

Default all manager responses to the language the user is currently using unless the user requests another language.

Codex prompts must explicitly require Codex to answer in the same language as the user.

For a Vietnamese-speaking non-technical user, include this block in substantial Codex prompts:

```text
YÊU CẦU NGÔN NGỮ:
- Trả lời hoàn toàn bằng tiếng Việt.
- Người dùng không có nền tảng lập trình.
- Giải thích bằng ngôn ngữ đơn giản, tập trung vào "nó làm gì" và "tại sao cần".
- Khi dùng thuật ngữ kỹ thuật tiếng Anh, giải thích ngắn gọn ngay lần đầu.
- Tên file, hàm, class, API, trạng thái và code có thể giữ nguyên tiếng Anh.
```

Do not translate identifiers in code or repository paths.

If the user switches language, follow the new language unless project instructions explicitly freeze a preference.

## 3. Anti-overengineering gate

Before approving a substantial architecture or implementation plan, answer these four questions:

1. **V1 need** — Is this complexity required by an accepted current requirement, or only by a hypothetical future need?
2. **Worst realistic consequence** — What actually happens if the simpler design is used?
3. **Simpler alternative** — Is there a smaller design that delivers most of the user value safely?
4. **Complexity value** — Is the added code, schema, state, testing, and maintenance justified by the consequence being avoided?

If the simpler design can fail only in a rare, recoverable, non-destructive way, prefer the simpler design unless the user explicitly approves the extra complexity.

Examples of consequences that usually justify extra complexity:

- realistic risk of permanent user-data loss or corruption;
- security/privacy exposure;
- irreversible destructive operations;
- frequent user-visible failure in the core workflow;
- an explicit accepted requirement that cannot be met simply;
- legal/compliance or external-system correctness requirements.

Examples that usually do **not** justify a large architecture by themselves in local V1:

- a rare duplicate valid backup file;
- an extra harmless retry after restart;
- cosmetic duplicate metadata that can be cleaned later;
- optimization for cloud/LAN/distributed deployment that is not in current scope;
- eliminating every theoretical crash window when the fallback is safe and recoverable.

## 4. Complexity escalation alarm

Stop before `/goal` and run a **Product Simplicity Review** when a small/medium feature begins to require several of these:

- a new global state machine;
- a scheduler or general-purpose job system;
- exact-once or distributed-style coordination;
- global revision/change-tracking across many tables;
- new migration(s) mainly to support orchestration rather than product data;
- several new persistence authorities or reconciliation layers;
- retry generations, fingerprints, attempt identities, or compensating transactions;
- broad changes across many stable subsystems for one narrow feature;
- large test matrices for crash states that have low real-world consequence;
- future cloud/LAN abstractions in a local-only V1.

These are warning signals, not automatic bans. The manager must compare them against the actual user consequence before proceeding.

## 5. Product Simplicity Review

Use this compact format before approving escalated complexity:

```text
PRODUCT SIMPLICITY REVIEW

Mục tiêu người dùng:
<one sentence>

Giải pháp tối thiểu khả thi:
<smallest design>

Thiết kế phức tạp đang đề xuất thêm:
<what extra machinery appears>

Nếu dùng giải pháp đơn giản, điều tệ nhất thực tế là gì?
<realistic consequence>

Mất dữ liệu / bảo mật / phá hủy không thể phục hồi?
Có / Không — <brief reason>

Khuyến nghị:
SIMPLE / BALANCED / ADVANCED

Lý do:
<plain-language explanation>
```

If the recommendation is ADVANCED only to eliminate a rare, recoverable edge case, do not proceed until the user explicitly approves that tradeoff.

## 6. Complexity budget

For local-first V1, prefer the lowest layer that meets the requirement safely:

```text
existing state / existing service
    ↓
one boolean or timestamp
    ↓
one small state row
    ↓
one narrow table
    ↓
domain-specific state machine
    ↓
general scheduler / orchestration system
    ↓
exact-once / distributed-style reconciliation
```

Do not jump to a lower layer merely because it is architecturally elegant.

Escalate only when the previous layer demonstrably cannot satisfy accepted safety or product behavior.

## 7. Plan-review simplicity checks

During `/plan` review, explicitly check:

- Can any proposed table, queue, state machine, retry engine, reconciliation layer, or background service be removed?
- Is the plan solving an observed problem or a hypothetical one?
- Is the plan preserving future optionality at the cost of current V1 complexity?
- Could the user safely accept a bounded imperfection instead?
- Is a simpler failure mode easier to understand, test, and recover from?
- Are tests proportional to user risk, or are they validating architecture created only to validate itself?

A technically correct plan can still receive `REVISE` for disproportionate complexity.

## 8. Non-technical decision output

When architecture tradeoffs matter, use this default structure:

1. **Bạn đang cố đạt điều gì?**
2. **Cách đơn giản nhất là gì?**
3. **Điều gì sẽ xảy ra nếu chọn cách đơn giản?**
4. **Có rủi ro mất dữ liệu/bảo mật không?**
5. **Tôi đề xuất chọn gì?**
6. **Bước tiếp theo duy nhất.**

Avoid leading with database/table/locking terminology.

## 9. Codex prompt requirements for non-technical users

Every substantial `/plan`, `/goal`, diagnose, review, or QA prompt should:

- include the Language Contract;
- ask Codex to add a short plain-language summary at the end;
- ask Codex to explain why a new architectural mechanism is necessary before proposing it;
- instruct Codex to prefer the smallest change consistent with accepted requirements;
- tell Codex not to add future-proof infrastructure without a current requirement;
- require Codex to call out when the implementation surface grows materially beyond the expected scope.

Suggested ending:

```text
Ở cuối báo cáo, thêm mục:
TÓM TẮT CHO NGƯỜI KHÔNG CÓ NỀN TẢNG CÔNG NGHỆ

Tối đa 10 dòng, giải thích:
- đã làm gì / đang đề xuất gì;
- vì sao cần;
- rủi ro thực tế còn lại;
- người dùng cần làm gì tiếp theo.
```

## 10. Safety boundary

Simplicity does not mean ignoring real safety.

Do not simplify away protections required to prevent:

- destructive Source writes;
- permanent canonical-data corruption;
- secret leakage;
- unsafe schema/data migration;
- irreversible file replacement;
- broken accepted recovery/restore behavior;
- high-likelihood race conditions in core workflows.

The goal is **minimum sufficient safety**, not maximum theoretical safety.
