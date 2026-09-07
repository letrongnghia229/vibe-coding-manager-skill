# Codex Prompt Templates — Compact by default

## Core rules

- Default to Vietnamese; preserve exact file/path/command/code/API/identifier/error/model names and precise English technical terms.
- Start with `AGENTS.md`, `docs/CURRENT_PHASE.md` when present, and the smallest relevant code/docs. Do not enumerate all project docs by default.
- `/plan` = inspect/plan only. `/goal` = implement approved plan from the same session.
- After `/plan` PASS, `/goal` references the approved plan instead of repeating it.
- Do not commit/tag/push before acceptance unless explicitly authorized.
- Do not start the next Round/Phase automatically.
- Ask for compact reports, focused verification, and STOP when repository reality conflicts materially with the approved plan.
- For blocker/STOP/invalidated assumption/architecture/data-safety/recovery/concurrency/security/destructive findings, add the `NON-TECHNICAL USER REPORTING` block below.

## Normal Round `/plan`

```text
/plan

[TÊN ROUND]

Đọc AGENTS.md, docs/CURRENT_PHASE.md nếu có, và chỉ code/docs trực tiếp liên quan.

MỤC TIÊU:
<một mục tiêu ngắn>

GAP / EVIDENCE:
<chỉ facts hiện tại cần cho Round>

ACCEPTANCE:
1. ...
2. ...

PLAN ONLY.
Trả về: current flow/root cause, smallest safe change, files, risks, compatibility/migration impact, tests.
Nếu phát hiện blocker/STOP/architecture-data-safety conflict hoặc assumption quan trọng sai, dùng NON-TECHNICAL USER REPORTING trước technical evidence.
Không sửa file. Không commit/tag/push. Không bắt đầu Round/Phase tiếp theo. Dừng để review.
Phản hồi hoàn toàn bằng tiếng Việt theo AGENTS.md.
```

## Plan correction delta

```text
/plan

CORRECTION DELTA ONLY

Giữ nguyên plan trước, ngoại trừ:
1. <correction>
2. <correction>

Chỉ trả: corrected flow/contract, tests/risks bị ảnh hưởng, và phần còn lại không đổi.
Nếu correction mở ra architecture/data-safety trade-off mới hoặc assumption cũ sai, dùng NON-TECHNICAL USER REPORTING.
PLAN ONLY. Không sửa file. Không commit/tag/push.
Phản hồi hoàn toàn bằng tiếng Việt theo AGENTS.md.
```

## Normal Round `/goal`

```text
/goal

[TÊN ROUND]

Triển khai approved plan từ chính session này.

REVIEW DELTA / GUARDRAIL:
- <chỉ correction/constraint mới; bỏ nếu không có>

VERIFICATION:
- focused tests cho hành vi vừa đổi;
- directly relevant regression tests;
- broaden only when Round risk requires;
- git diff --check.

Nếu repository reality mâu thuẫn đáng kể với approved plan, STOP và báo cáo trước khi tự mở rộng scope.
Nếu gặp blocker/STOP/invalidated assumption/architecture-data-safety conflict, dùng NON-TECHNICAL USER REPORTING trước technical evidence.
Không commit/tag/push. Không bắt đầu Round/Phase tiếp theo.
Báo cáo ngắn: files changed, behavior changed, exact PASS/FAIL, deviation/risk, manual QA.
Không lặp lại plan hoặc paste diff/log dài.
Phản hồi hoàn toàn bằng tiếng Việt theo AGENTS.md.
```

## Diagnose only

```text
DIAGNOSE ONLY

VẤN ĐỀ:
<actual behavior>

EVIDENCE:
<screenshots/log/runtime facts liên quan>

Inspect phạm vi nhỏ nhất trước. Tách:
- CONFIRMED
- LIKELY
- UNKNOWN
- next action nhỏ nhất

Nếu phát hiện blocker/STOP/data-safety/recovery/security/concurrency/architecture conflict hoặc assumption quan trọng sai, bắt đầu bằng NON-TECHNICAL USER REPORTING.
Không sửa code/data. Không commit/tag/push.
Phản hồi hoàn toàn bằng tiếng Việt theo AGENTS.md.
```

## Confirmed Fix Round `/plan`

```text
/plan

[FIX ROUND]

CONFIRMED GAP:
<root cause/evidence>

REQUIRED FIX:
<correction nhỏ nhất cần có>

PRESERVE:
<accepted behavior có nguy cơ regression>

PLAN ONLY.
Trả về: exact fix point, smallest change, files, tests, risks.
Nếu fix cần architecture/dependency mới, thay đổi data-safety contract, hoặc complexity tăng nhiều so với gap gốc, dùng NON-TECHNICAL USER REPORTING và STOP để review trước implementation.
Không sửa file. Không commit/tag/push. Không bắt đầu Phase tiếp theo.
Phản hồi hoàn toàn bằng tiếng Việt theo AGENTS.md.
```

## Confirmed Fix Round `/goal`

```text
/goal

Triển khai approved Fix Round plan từ chính session này.

REVIEW DELTA:
- <chỉ guardrail mới, nếu có>

Thêm focused regression test khi phù hợp.
Chạy focused + direct regression tests; broaden only if risk requires.
Nếu approved assumption sai hoặc cần mở rộng architecture đáng kể, STOP và dùng NON-TECHNICAL USER REPORTING.
Không commit/tag/push. Dừng để user retest acceptance step đã fail.
Báo cáo ngắn, không lặp lại plan.
Phản hồi hoàn toàn bằng tiếng Việt theo AGENTS.md.
```

## Independent review

```text
INDEPENDENT REVIEW — NO EDITS

[TÊN ROUND / PHASE]

Dùng current local worktree làm authority cho uncommitted work.
Review correctness/canonical flow, migration/data safety, persistence/recovery/concurrency/destructive/security risks khi liên quan, scope/dead complexity, tests/regressions.

Trả về:
VERDICT: PASS hoặc REVISE
BLOCKERS: <NONE hoặc numbered>
NON-BLOCKING: <ngắn>
TEST EVIDENCE: <exact commands/results>

Nếu có blocker HIGH/CRITICAL hoặc architecture/data-safety conflict, bắt đầu bằng NON-TECHNICAL USER REPORTING.
Không block vì style. Không sửa file. Không commit/tag/push.
Phản hồi hoàn toàn bằng tiếng Việt theo AGENTS.md.
```

## Final Regression Review

```text
/plan

PHASE [N] — FINAL REGRESSION REVIEW

PREVIOUS STABLE CHECKPOINT: <tag/commit>
CURRENT: all planned Rounds accepted; local worktree contains current uncommitted Phase work.

REVIEW ONLY.
Review stable checkpoint -> current worktree for real blockers: regressions, duplicate authority, migration/data safety, persistence/recovery, concurrency/races, destructive filesystem/source safety, security, user-data preservation, missing critical tests, complexity threatening stability.

Trả về:
A. confirmed-correct areas
B. blockers
C. non-blocking observations
D. exact final verification set
E. READY FOR FINAL VERIFICATION hoặc MUST FIX BEFORE CHECKPOINT

Nếu có HIGH/CRITICAL blocker, prepend NON-TECHNICAL USER REPORTING: practical issue, worst case, damage confirmed hay only risk, và vì sao checkpoint phải dừng.
Không sửa file. Không commit/tag/push. Không bắt đầu Phase tiếp theo.
Phản hồi hoàn toàn bằng tiếng Việt theo AGENTS.md.
```

## Final Verification

```text
/goal

PHASE [N] — FINAL VERIFICATION

Chỉ chạy approved final verification set.
Không thêm feature/refactor ngoài scope.
Báo cáo exact PASS/FAIL, git diff --check, working-tree status.
Nếu phát hiện high-risk blocker, prepend NON-TECHNICAL USER REPORTING.
Không commit/tag/push nếu final manual acceptance chưa PASS.
Dừng cho final manual smoke.
Phản hồi hoàn toàn bằng tiếng Việt theo AGENTS.md.
```

## Final Delta Verification

```text
FINAL DELTA VERIFICATION

Verify only the narrow post-review fix + direct regression surface.
Nếu fix chạm migration/schema, canonical data, persistence/recovery, concurrency/job lifecycle, destructive filesystem, security hoặc broad architecture, STOP và yêu cầu broader final review/verification.
Nếu STOP vì risk rộng hơn, dùng NON-TECHNICAL USER REPORTING.
Không commit/tag/push.
Phản hồi hoàn toàn bằng tiếng Việt theo AGENTS.md.
```

## Checkpoint

```text
FINAL MANUAL ACCEPTANCE: PASS

Tạo checkpoint: <TAG>

Nếu required checks PASS:
- update checkpoint/release docs;
- commit accepted Phase changes;
- tạo exact tag;
- push intended branch + tag;
- verify remote commit/tag;
- verify clean local working tree.

Nếu có bước FAIL, STOP trước khi tuyên bố checkpoint hoàn tất.
Không bắt đầu Phase tiếp theo.
Phản hồi hoàn toàn bằng tiếng Việt theo AGENTS.md.
```

## Compact implementation report

```text
Báo cáo ngắn:
1. files changed;
2. behavior changed;
3. exact tests/builds + PASS/FAIL;
4. deviations/blockers/known risks;
5. manual QA.

Nếu mục 4 có blocker/STOP/high-risk invalidated assumption, đặt NON-TECHNICAL USER REPORTING trước report kỹ thuật.
Không lặp plan hoặc paste diff/log dài.
```

## NON-TECHNICAL USER REPORTING

Add this block to high-risk prompts only when relevant:

```text
NON-TECHNICAL USER REPORTING

Nếu phát hiện blocker, STOP condition, invalidated assumption, architecture conflict,
migration/data-safety issue, recovery/concurrency/security/destructive risk:

Bắt đầu report bằng:

## Giải thích cho người không có nền tảng kỹ thuật
- Vấn đề là gì? 1–2 câu rất đơn giản.
- Nếu cơ chế phức tạp: cho ít nhất 1 ví dụ/so sánh đời thường dễ hiểu.
- Hậu quả thực tế xấu nhất là gì?
- Có dữ liệu/hành vi nào đã thực sự bị ảnh hưởng chưa, hay hiện chỉ là nguy cơ?
- Tại sao cần STOP/đổi plan/Fix Round?
- User có decision product/workflow thật sự cần đưa ra không? Nếu không, nói rõ không cần user chọn chi tiết kỹ thuật.

Sau đó mới có:

## Technical evidence
<evidence chính xác, ngắn>
```

Rules:
- Never call potential risk confirmed damage.
- The simple explanation does not replace technical evidence.
- Do not force the user to choose low-level implementation details with no product trade-off.
- If solution complexity grows materially beyond the original bug, state that and STOP for ChatGPT/user review before expanding architecture.

## Codex Prompt Preflight

Before emitting any handoff, silently check:

```text
[ ] Am I repeating stable rules already in AGENTS.md?
[ ] Am I asking Codex to read docs without a concrete reason?
[ ] Does /goal repeat the approved /plan?
[ ] Can targeted inspection/tests answer this before broad work?
[ ] Is verification proportionate to risk?
[ ] Is the requested report compact?
[ ] For a possible blocker/STOP/high-risk finding, is non-technical reporting requested?
```

Keep the prompt to the smallest sufficient NOW/delta/evidence plus required safety/stop rules.
