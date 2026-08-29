# Specification: Teacher-Controlled Analytical Learning and MCP Reporting

## Problem Statement

จากมุมมองของครู นักเรียนจำนวนหนึ่งส่งการบ้านที่ไม่แสดงกระบวนการคิดของตนเอง เพราะสามารถใช้ generative AI สร้างคำตอบหรือคัดลอกคำตอบได้ ครูจึงแยกได้ยากว่าเด็กเข้าใจบทเรียนจริงหรือไม่ และใช้เวลามากในการออกโจทย์ ตรวจคำตอบ ให้ feedback และติดตามความเข้าใจรายบุคคล

จากมุมมองของนักเรียน งานการบ้านแบบคำตอบสุดท้ายอย่างเดียวไม่ได้ช่วยฝึกการตั้งปัญหา การใช้หลักฐาน การให้เหตุผล การประเมินทางเลือก หรือการแก้ไขความคิดเมื่อมีข้อมูลใหม่ เด็กที่พื้นฐานต่างกันยังได้รับระดับความช่วยเหลือเดียวกัน

จากมุมมองของโรงเรียน การใช้ AI กับข้อมูลเด็กมีความเสี่ยงด้านความเป็นส่วนตัว ความไม่เป็นธรรม และการกล่าวหาผิดพลาด ระบบจึงต้องไม่เปลี่ยน similarity หรือ detector output ให้เป็นคำตัดสินว่าเด็กโกงโดยอัตโนมัติ

ประเทศไทยยังไม่มีหลักฐานระดับประเทศที่น่าเชื่อถือสำหรับอัตรา GenAI plagiarism ใน K–12 จึงต้องวัด authentic understanding และ process evidence แทนการอ้างตัวเลขการโกงที่ไม่มีฐานข้อมูลรองรับ

## Solution

สร้างแพลตฟอร์มที่ให้ครูสร้างหรือป้อน lesson objective และ rubric แล้วให้ AI ร่าง analytical-thinking assignment ครูตรวจและ approve ก่อนปล่อยให้นักเรียนทำ

ระบบจะเก็บ first attempt, เหตุผล, หลักฐาน/สมมติฐาน, hint events และ revision เพื่อประเมินกระบวนการคิด ให้ AI feedback ที่ผูกกับ rubric และจัดนักเรียนเป็น temporary skill-specific action group ที่เปลี่ยนได้ตามหลักฐาน ไม่ใช้ label ถาวรหรือจัดอันดับเด็ก

Pilot เริ่มที่นักเรียน ป.5 วิทยาศาสตร์ หน่วยระบบนิเวศและสิ่งแวดล้อม ในห้องเรียนของครูหนึ่งคน ระบบรองรับทุกวิชาและมัธยมใน architecture แต่ยังไม่อ้างผลลัพธ์นอก pilot

ระบบมี multi-model overlap signal โดยทดสอบโจทย์กับหลายโมเดลก่อนปล่อย และใช้ผลความคล้ายหลังส่งงานเป็นข้อมูล private สำหรับครูเท่านั้น Signal ใช้เปิด review ไม่ใช้หักคะแนนหรือกล่าวหาอัตโนมัติ

เพิ่ม official MCP server สำหรับให้ครูเชื่อมกับ Claude Cowork, ChatGPT หรือ provider ที่รองรับ เพื่ออ่าน aggregate class insight และสร้าง draft report โดยเริ่มจาก read-only tools บน Heroku ผ่าน HTTPS ห้ามส่ง raw student data หรือ PII ไป external provider ใน MVP

## User Stories

### นักเรียน

1. As a ป.5 student, I want to receive an assignment based on the lesson I learned, so that the task is relevant to class content.
2. As a student, I want an assignment that asks me to use evidence and explain reasoning, so that I practise thinking rather than recalling an answer.
3. As a student, I want to identify what I know and do not know before receiving a hint, so that I try to solve the problem myself.
4. As a student, I want hints to become more specific only after I attempt the task, so that support does not turn into an answer.
5. As a student, I want hints to use concepts from my lesson, so that the help is understandable and relevant.
6. As a student, I want the assistant not to reveal the final answer, so that my submitted work represents my own understanding.
7. As a student, I want to submit a first attempt, so that my initial reasoning is visible as learning evidence.
8. As a student, I want to record evidence and assumptions, so that I can distinguish facts from guesses.
9. As a student, I want to revise my answer after feedback, so that I learn from errors instead of only seeing a score.
10. As a student, I want to see the next learning task without seeing a stigmatizing ability label, so that grouping does not define me.
11. As a student, I want to complete work on a mobile device with low bandwidth, so that access constraints do not prevent participation.
12. As a student, I want my work to autosave, so that an interrupted connection does not erase my progress.
13. As a student, I want feedback to explain the next step using evidence from my answer, so that I know how to improve.
14. As a student, I want to report that feedback is incorrect or unclear, so that an AI mistake can be corrected.
15. As a student, I want my learning data to be handled transparently and deletable where applicable, so that I retain agency over my records.
16. As a student, I want any concern about my work to be reviewed by a teacher rather than decided by an algorithm, so that I can explain my process.

### ครู

17. As a teacher, I want to enter lesson objectives and approved learning material, so that generated assignments stay within the taught content.
18. As a teacher, I want AI to draft an assignment with a rubric, so that I spend less time starting from a blank page.
19. As a teacher, I want to edit, reject, or approve every assignment before release, so that AI does not publish unchecked content.
20. As a teacher, I want subject-specific rubric templates, so that analytical thinking is assessed appropriately for each subject.
21. As a teacher, I want to see whether an assignment requires evidence, reasoning, evaluation, and transfer, so that I can judge its analytical depth.
22. As a teacher, I want to approve the feedback rules and hint schema, so that the assistant follows my instructional intent.
23. As a teacher, I want newly generated hints that violate the schema to enter a review queue, so that unsupported or overly revealing hints are not shown automatically.
24. As a teacher, I want to see first attempts, evidence, revisions, and hint events, so that I can assess authentic understanding.
25. As a teacher, I want AI feedback to be tied to rubric evidence, so that fluent but unsupported feedback is not shown as authoritative.
26. As a teacher, I want to override a recommended action group, so that contextual knowledge about a student remains part of the decision.
27. As a teacher, I want action groups to be temporary and skill-specific, so that a past misconception does not become a permanent label.
28. As a teacher, I want to see the evidence and uncertainty behind a group recommendation, so that I can review it responsibly.
29. As a teacher, I want to reassess students after a short learning cycle, so that group membership changes when evidence changes.
30. As a teacher, I want to run a multi-model preflight on an assignment, so that I can identify tasks that a general-purpose model can answer too easily.
31. As a teacher, I want to see model-overlap evidence privately, so that I can decide whether additional review is needed without exposing an accusation to a student.
32. As a teacher, I want to request a short explanation or additional evidence from a student, so that a signal can lead to a fair review conversation.
33. As a teacher, I want no detector signal to change a grade or group automatically, so that false positives do not harm students.
34. As a teacher, I want to see class-level learning trends, so that I can plan reteaching and extension activities.
35. As a teacher, I want to generate a draft class report through an AI provider, so that I can prepare documentation without manually copying aggregate data.
36. As a teacher, I want to see which provider, class scope, fields, and time period a report uses, so that I understand what leaves the platform.
37. As a teacher, I want to disconnect a provider and revoke access, so that an integration is not permanent.
38. As a teacher, I want to delete eligible student records, drafts, and hint logs, so that retention follows the stated policy.
39. As a teacher, I want the system to remain usable when the provider is unavailable, so that core teaching is not blocked by an external AI service.

### ผู้บริหารโรงเรียนและผู้ดูแลระบบ

40. As a school administrator, I want class-scoped teacher authorization, so that a teacher cannot access another class.
41. As a school administrator, I want reports to suppress small groups, so that aggregate data cannot identify individual children.
42. As a school administrator, I want an audit record for provider connections, report generation, exports, and review decisions, so that access is accountable.
43. As a school administrator, I want no student PII in external MCP payloads by default, so that the integration starts with a safer data boundary.
44. As a school administrator, I want external report generation to produce a draft rather than publish or send automatically, so that a human checks accuracy and recipients.
45. As a school administrator, I want write actions disabled in the initial MCP integration, so that an external model cannot change grades, grouping, feedback, or roster data.
46. As a school administrator, I want provider compatibility and retention terms recorded per connection, so that “MCP-compatible” is not treated as a universal safety guarantee.
47. As a school administrator, I want a kill switch for a custom MCP server, so that a compromised or unsafe connector can be disconnected quickly.
48. As a school administrator, I want the product to fail closed when authorization or redaction cannot be verified, so that uncertainty does not become data exposure.

### ผู้พัฒนาและผู้ประเมินผล

49. As a product evaluator, I want a no-AI parallel transfer task before and after the pilot, so that learning gain is not confused with polished AI-assisted answers.
50. As a product evaluator, I want delayed retention measurement, so that short-term completion is not treated as durable learning.
51. As a product evaluator, I want results disaggregated by access mode, so that the product does not hide device-related inequity behind an average.
52. As a product evaluator, I want teacher review time and correction burden measured, so that personalization is not achieved by adding unsustainable work.
53. As a product evaluator, I want model-overlap false-positive review and appeals measured, so that more flags are not mistaken for more integrity.
54. As a product evaluator, I want provider-specific MCP tests, so that a working connection on one client is not assumed to work on another.

## Implementation Decisions

### Product scope and workflow

- The product vision supports primary, secondary, and multiple subjects. The first validated content is one ป.5 science unit on ecosystems and environment.
- The primary user is a teacher operating one class. The student experience is mobile-first and low-bandwidth with autosave.
- The core workflow is: teacher prepares lesson objective and material; AI drafts assignment/rubric; teacher reviews and approves; student attempts; hint/feedback is delivered; student revises; the system updates skill evidence and recommends the next action; the teacher reviews exceptions.
- Assignment lifecycle is draft, teacher review, released, active, submitted, feedback, revised, and closed. AI-generated assignment or rubric content cannot move from draft to released without teacher approval.

### Assessment model and KOI

- Each assignment maps to five subject-grounded dimensions: problem framing, evidence and assumptions, reasoning chain, evaluation and revision, and transfer and explanation.
- Each dimension is scored from 0 to 3 with observable anchors. The score must not reward grammar, writing polish, accent, or speed as a proxy for critical thinking.
- The launch science template uses weights of 15%, 30%, 30%, 15%, and 10% respectively. Math, Thai/Social, and English use the subject templates captured in the research/design decision; teachers may adjust within a controlled range rather than freely redefining the scale.
- The weighted score is normalized to 0–100.
- The primary KOI is the within-student change in a parallel, no-AI transfer task: post-learning transfer score minus pre-learning transfer score.
- Pilot reporting uses median gain, distribution, dimension-level scores, delayed retention, independent retry after hints, and disaggregation by access mode.
- The provisional product target is a median gain of at least 10 percentage points. This is a hypothesis for the pilot, not a national or research benchmark.
- Secondary KOIs cover hint efficacy, reassessment/group movement, teacher review minutes, teacher correction burden, integrity-signal review/appeal outcomes, and access-mode completion/learning gaps.

### Action groups

- The system uses temporary, skill-specific action groups: Diagnose/Reteach, Guided Practice, and Independent Transfer.
- Grouping is based on evidence for a knowledge component, not a permanent overall ability label.
- Initial operating rules are: repeated 0–1 evidence on a core skill indicates reteach; partial or prompt-dependent performance indicates guided practice; demonstrated 2+ performance across two observations plus a successful transfer check permits independent transfer.
- The system reassesses after approximately 3–5 items and one explanation. It records evidence, uncertainty, reassignment, and teacher override.
- Students see the next learning task, not the group name. Group membership cannot affect grades, discipline, admission, or access to opportunity.

### Hint and feedback policy

- AI may generate hints but only from teacher-approved lesson material, rubric, assignment context, and a fixed hint schema.
- The hint ladder moves from identifying the stuck point, to concept cue, to strategy cue, to a different-context micro-example, to checking part of a process.
- The assistant must not provide the final answer, complete the actual problem, rewrite a student response into a finished submission, or use a high-level hint before an attempt.
- Hints generated outside the approved schema enter a teacher review queue. Repeated hint requests, abandonment, or answer-copy patterns are support signals, not misconduct findings.
- Every student submits a first attempt, a short explanation/evidence record, and a revision. A teacher may request additional explanation or an oral/teacher-led check for a review case; oral defense is not mandatory for every assignment.
- Feedback is released only when tied to rubric evidence and within the approved rule/template. Feedback identifies a strength, a missing element, and one next question; it must not write the answer for the student.

### Multi-model overlap signal

- Before release, the system may test a teacher-approved assignment against selected popular models to measure AI solvability and analytical depth.
- After submission, the system may compare the student's work with stored model outputs as a private review signal. It records provider/model/version, timestamp, comparison method, and overlapping spans.
- The signal is named Multi-model overlap signal. It is not an authorship verdict or cheating probability.
- Only teachers or authorized school reviewers see the signal. It never automatically changes a grade, group, discipline record, admission decision, or student-facing status.
- A high signal opens a private review path using process evidence and optional additional explanation. The system records the teacher decision and appeal/correction outcome.
- Student answers and personally identifying data are not sent to external models for post-submission detection in the MVP. Preflight requests contain no student data.

### Subject templates

- Science: framing 15, evidence 30, reasoning 30, evaluation 15, transfer 10.
- Mathematics: framing 10, evidence 20, reasoning 40, evaluation 20, transfer 10.
- Thai/Social Studies: framing 20, evidence 30, reasoning 25, evaluation 15, transfer 10.
- English: framing 15, evidence 20, reasoning 25, evaluation 15, transfer/explanation 25; language mechanics are scored separately from reasoning and cannot dominate the reasoning score.
- Each template has teacher-visible rubric anchors and a controlled adjustment range. Rubric changes are versioned so scores remain interpretable.

### MCP reporting layer

- The first MCP integration is an official platform MCP server deployed as a public HTTPS service on Heroku. Heroku CLI is an accepted deployment path; it is not treated as an authorization or privacy boundary.
- The normal teacher path is configuration and connection, not arbitrary server coding. Custom MCP servers may be deployed by an approved technical/admin user in a later phase, but they cannot access the product database directly.
- The MCP server implements the conservative intersection of supported provider capabilities: authenticated remote tools over HTTPS, stable JSON schemas, bounded results, read-only operations, and optional draft report generation. Provider/version compatibility is tested separately for each client.
- Initial tools are class-summary retrieval, learning-objective retrieval, rubric-template retrieval, group-trend retrieval, and draft-class-report generation. No arbitrary SQL, filesystem, URL fetch, or generic execute tool is exposed.
- MCP responses contain only class-level aggregates, bounded time periods, rubric/objective metadata, and teacher-approved context. Small-cell suppression is applied; raw answers, names, student IDs, free text, individual integrity flags, grades, attendance, health/family data, discipline, and full rosters are denied by default.
- `draft-class-report` creates an uncommitted draft. External AI cannot write assignment, feedback, grouping, grade, roster, attendance, message, or deletion data in the demo.
- Production authorization uses per-user consent, class-scoped grants, short-lived audience-bound tokens, PKCE/OAuth discovery as required by the client, issuer/audience validation, revocation, rate limits, pagination, result-size caps, and redacted audit events. Shared service tokens and token passthrough are prohibited.
- Provider connection UI shows provider, server identity, protocol version, class scope, fields leaving the platform, retention/terms status, and approval state. The teacher can disconnect and revoke access.
- MCP report drafts remain separate from grades, discipline, admission, and access decisions. Core learning workflows continue if an external provider is unavailable.

### Data lifecycle and governance

- Use pseudonymous internal learner identifiers and collect only data required for stated learning purposes.
- Student data, drafts, hint events, feedback, grouping evidence, integrity signals, reports, and audit metadata have explicit retention categories.
- Authorized users can delete eligible records. Deletion removes active content and queued exports; security/audit records retain only the minimum non-content metadata required for accountability.
- Provider retention, residency, subprocessors, training use, and deletion behavior are recorded per provider and are not inferred from MCP compatibility.
- Any external payload is redacted before transmission. If redaction or scope validation fails, the request fails closed.

## Testing Decisions

- The highest test seam is one end-to-end teacher-class workflow: approve a lesson-based assignment → student submits first attempt → hint/feedback is delivered → student revises → rubric score and action-group recommendation update → teacher reviews any integrity signal → teacher creates an aggregate MCP draft report. This seam validates the visible product contract without coupling tests to internal module structure.
- No existing application code, test suite, domain glossary, or ADR was present in the repository. The test plan therefore establishes new contract-level coverage rather than copying prior test patterns.
- Tests must assert observable behavior, state transitions, authorization boundaries, redaction, and user-visible outcomes. They must not assert prompt wording, model internals, private implementation classes, or incidental UI structure.
- Assessment tests cover 0–3 rubric anchors, weighted 0–100 calculation, subject-template selection, missing evidence, revision, parallel transfer scoring, and exclusion of language polish/speed from critical-thinking score.
- Grouping tests cover repeated weak evidence, partial mastery, successful transfer, reassignment after reassessment, teacher override, uncertainty, student-facing neutral task language, and prohibition on grade/discipline side effects.
- Hint/feedback tests cover attempt-before-hint, each hint tier, no final-answer leakage, schema rejection/review queue, lesson grounding, rubric-linked feedback, feedback correction, and interrupted/mobile autosave behavior.
- Integrity tests cover model-specific overlap display, teacher-only visibility, no automatic penalty or group movement, private review, appeal/correction recording, empty or conflicting model outputs, and the rule that no student PII or post-submission answer is sent to external models in the MVP.
- MCP tests cover user consent, class-bound authorization, cross-class denial, token audience validation, revocation, small-cell suppression, redaction, result bounds, provider outage/fail-closed behavior, draft-only report creation, disabled write tools, audit events, deletion behavior, and provider-specific transport/version compatibility.
- A pilot evaluation must use pre/post parallel no-AI transfer tasks, delayed retention, teacher double-scoring on a calibration sample where feasible, access-mode disaggregation, and teacher time/correction-burden measurement.
- MCP compatibility should be exercised against each target client rather than inferred from one successful connection. The minimum target is the conservative intersection of remote Streamable HTTP and any legacy transport required by a documented client.

## Out of Scope

- Production-ready content for every grade, every subject, or all Thai curricula beyond the ป.5 science pilot.
- A general-purpose student chatbot or unrestricted answer-generation assistant.
- Automatic AI-authorship detection, cheating probability, plagiarism verdicts, automatic penalties, or disciplinary automation.
- Sending raw student submissions, names, student IDs, free text, or sensitive child data to ChatGPT, Claude, Gemini, or another external provider in the MVP.
- Using model-overlap signals for grades, discipline, admissions, permanent grouping, or opportunity restriction.
- Fully automatic release of generated assignments, hints, feedback, or reports without the defined teacher controls.
- External MCP write actions, grade changes, roster changes, attendance, messages, deletion, scheduled unattended jobs, or report sending to families/students in the demo.
- Arbitrary teacher-built MCP servers with direct database access. Custom servers require a later approval, sandbox, provenance, egress, and kill-switch process.
- Paper/offline teacher-entered workflow in the MVP; the initial access commitment is mobile-first, low-bandwidth, and autosave.
- A causal claim that the product improves national PISA performance or that AI causes Thai students’ learning problems.
- Treating PISA, policy targets, provider documentation, or institutional guidance as proof of product efficacy or national GenAI-plagiarism prevalence.

## Further Notes

- Thailand research note: `docs/research-thailand-ai-education.md`.
- Measurement and grouping research note: `docs/research-measurement-grouping.md`.
- MCP compatibility research note: `docs/research-mcp-provider-compatibility.md`.
- The research supports treating device access as heterogeneous, measuring reasoning with subject-grounded tasks, keeping action groups reversible, and using integrity signals as prompts for human review rather than verdicts.
- MCP protocol and security references: https://modelcontextprotocol.io/specification/2026-07-28 and https://modelcontextprotocol.io/docs/2026-07-28/tutorials/security/security_best_practices.
- Provider compatibility references include Anthropic remote connectors/Cowork documentation and OpenAI developer-mode/MCP documentation. Provider support, retention, and plan availability are time-sensitive and must be revalidated before release.
- The repository is not a Git checkout and no GitHub repository context or issue-tracker label vocabulary is available in this session. The local specification is complete, but publishing it with the `ready-for-agent` label is blocked until `/setup-matt-pocock-skills` supplies the issue-tracker context.
