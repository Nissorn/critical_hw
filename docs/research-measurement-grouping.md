# Measurement and grouping research note

**Purpose.** Evidence and a draft measurement design for a Thai education product that (a) lets teachers approve AI-generated analytical-thinking homework, (b) gives formative feedback, (c) recommends temporary support groups, and (d) shows teachers academic-integrity signals. This note separates **sourced findings** from **recommendations/inferences**. It is not a validation of the product or legal advice.

## 1. What should count as analytical/critical thinking?

### Sourced findings

- **The OECD operationalizes reasoning in a subject-grounded construct rather than a single generic “critical-thinking score.”** The OECD’s *PISA 2022 Assessment and Analytical Framework* (OECD, 31 August 2023) defines mathematical literacy as the capacity to reason mathematically and formulate, employ and interpret mathematics to solve real-world problems; it includes the well-founded judgements and decisions citizens need. Its reasoning actions include identifying, connecting and representing; constructing, evaluating, deducing, justifying and explaining; and interpreting, judging, critiquing, refuting and qualifying. The framework allocates assessment evidence across mathematical reasoning and the processes of formulating, employing, and interpreting/applying/evaluating outcomes. [OECD, *PISA 2022 Assessment and Analytical Framework*, 2023, mathematics framework](https://www.oecd.org/en/publications/pisa-2022-assessment-and-analytical-framework_dfe0bf9c-en/full-report/component-3.html) (see especially §§ “Mathematical reasoning” and “Problem solving”).
- The same framework says reasoning also involves evaluating interpretations and inferences, and that interpreting/evaluating includes deciding whether a result is reasonable in context. It explicitly cautions that students need domain content knowledge to reason and solve contextual problems. This supports scoring reasoning *within a subject context*, rather than claiming that a short generic quiz measures transferable critical thinking. [OECD, 2023, same source](https://www.oecd.org/en/publications/pisa-2022-assessment-and-analytical-framework_dfe0bf9c-en/full-report/component-3.html).
- PISA 2022’s creative-thinking framework (OECD, 2023) uses an evidence-centred design process: define the domain and construct, specify claims, identify observable evidence, design/validate tasks, and assemble enough tasks to support claims. Its creative-thinking definition concerns generating, evaluating and improving ideas; this is adjacent to, but not identical with, critical evaluation. The framework describes creativity as multidimensional and dependent on knowledge, motivation, environment and domain resources. [OECD, *PISA 2022 Assessment and Analytical Framework*, 2023, creative-thinking framework](https://www.oecd.org/en/publications/pisa-2022-assessment-and-analytical-framework_dfe0bf9c-en/full-report/component-5.html).

### Recommendation / inference for this product

Use a **claim–evidence–task** blueprint per subject/unit. A defensible analytical-thinking rubric (0–3 per dimension) could score:

1. **Problem framing:** identifies the actual question, constraints and relevant information.
2. **Evidence and assumptions:** selects relevant evidence, distinguishes fact from assumption, and acknowledges uncertainty or missing information.
3. **Reasoning:** links evidence to a conclusion with a coherent chain; uses an appropriate model, method or counterexample.
4. **Evaluation:** checks alternatives, limitations, bias or plausibility; revises when evidence conflicts.
5. **Communication/transfer:** explains the judgement clearly and applies the reasoning to a novel but structurally related case.

These are a **proposed operationalisation**, not OECD-defined universal subscales. Keep subject content and language demands visible in scoring. Require at least one unfamiliar transfer task and a short student explanation of reasoning; otherwise the product may mostly measure recall, fluent Thai/English writing, or ability to prompt an AI.

**Practical assessment design:** teacher approves a task against the rubric before release; collect the initial answer, evidence/assumptions, revision, and a brief post-task explanation. Use multiple tasks and parallel forms rather than one score. Double-score a calibration sample with an independent teacher and monitor agreement; low agreement is a measurement problem, not a student problem.

## 2. Formative assessment and mastery grouping

### Sourced findings

- *Formative Assessment: Improving Learning in Secondary Classrooms* (OECD, edited/project-managed by Janet Looney, publication date 24 January 2005) describes formative assessment as frequent assessment of progress used to identify learning needs and shape teaching. It reports high achievement gains attributed to formative assessment but also says practice was not systematic. [OECD, 2005](https://www.oecd.org/en/publications/formative-assessment_9789264007413-en.html), DOI [10.1787/9789264007413-en](https://doi.org/10.1787/9789264007413-en).
- An OECD case study, *Teaching, Learning and Assessment for Adults: Improving Foundation Skills—Case Study: Belgium (Flemish Community)* (David J. Rosen and Inge De Meyer, 2008), describes diagnostic tests, individual trajectory plans that can change during learning, teacher logs, dialogue and peer learning. It reports grouping learners with the same goals/level where possible while retaining individually paced materials when group composition changes. This is an illustrative case, not a causal trial or guarantee of effect. [OECD, 2008 PDF](https://web-archive-storage.oecd.org/aemint-web-archive-prod/web-archive/55/5564669b87cb3d000e2d3e40c901668c638719270c9aafecc8c154b1df759995.pdf).
- *PISA 2022 Results (Volume II), Chapter 4: Selecting and grouping students* (OECD, 2023) reports that ability grouping can let teachers tailor instruction, but also notes that equitable/high-performing systems generally have less between-class ability grouping. Across OECD countries, the average mathematics difference associated with schools using within-class ability grouping versus not using it was small and favoured schools without grouping by about three points after socio-economic adjustment; the report warns that these cross-sectional relationships do not justify causal inference. [OECD, 2023](https://www.oecd.org/en/publications/pisa-2022-results-volume-ii_a97db61c-en/full-report/component-11.html), §§ “Ability grouping” and “Components of resilience.”
- Bayesian Knowledge Tracing (BKT) is an original learner-modelling method: Albert T. Corbett and John R. Anderson, “Knowledge tracing: Modeling the acquisition of procedural knowledge,” *User Modeling and User-Adapted Interaction* 4 (1995), DOI [10.1007/BF01099821](https://doi.org/10.1007/BF01099821). In a later technical review, Aleven, McLaughlin, Glenn and Koedinger define adaptivity as responding to domain demands, psychological measures and learner actions; they describe Cognitive Tutor task selection as estimating knowledge dynamically and selecting practice for knowledge components not likely mastered. [Aleven et al., *Instruction Based on Adaptive Learning Technologies*, Carnegie Mellon University pre-publication copy, 2016](https://www.cs.cmu.edu/~aleven/Papers/2016/Aleven_etal_Handbook2016_AdaptiveLearningTechnologies_Prepub.pdf). The pre-publication review also says evidence for next-step hints is positive but limited/equivocal in places, and that machine-learned policies have not clearly beaten strong non-adaptive controls in all settings.

### Concrete grouping method (recommendation / inference)

Use **temporary, skill-specific action groups**, not fixed learner labels:

1. Map every diagnostic and homework criterion to a small set of explicit knowledge components (for example, “identify a confounder” or “justify a proportional model”).
2. Maintain, per learner and component, a mastery probability plus uncertainty. BKT is one possible model, but it must be locally calibrated; do not treat its probability as a psychological truth.
3. Assign the next instructional action from the evidence, not a permanent rank: (a) *diagnose/reteach* when evidence is weak or uncertain, (b) *guided practice* when partial mastery is likely, (c) *independent transfer* when mastery is supported. These names are internal workflow states; students should see a neutral task goal, not “low” or “high” ability.
4. Form small groups by the **same next misconception/goal**, optionally mixing other strengths for peer explanation. Recompute after each short cycle (for example, 3–5 items plus one explanation), and move learners when new evidence changes the estimate. A teacher can override, pause or dissolve a group.
5. Require at least two informative observations or a teacher confirmation before assigning a remediation action; log the evidence and model uncertainty. Keep a non-digital/teacher-entered path for learners with intermittent access.

**Caveats.** The thresholds and cycle length are product hypotheses, not sourced cut scores. A BKT model can be wrong when items are mis-tagged, answers are copied, language or device constraints intervene, or the task measures several skills at once. Measure reassessment accuracy, movement between groups, teacher overrides and learning on an independent transfer task. Never use group membership for grades, discipline, admissions or opportunity restriction. OECD’s cross-sectional PISA results are a reason to keep groups fluid and low-stakes, not evidence that all grouping is harmful.

## 3. Adaptive hints and support policies

### Sourced findings

- Aleven et al. (2016) distinguish **task-loop** adaptation (choose the next task) from **step-loop** adaptation (respond to an action within the task). They describe evidence that dynamic task selection based on evolving knowledge can improve effectiveness/efficiency, while results for step-level adaptation and next-step hints are more mixed. The review gives an important expertise-reversal warning: structured adaptive support helped low-prior-knowledge learners in one self-explanation study, while more knowledgeable learners needed less structure and could be impeded by it. [Aleven et al., 2016, pp. 2–5, 12–16, 21–25](https://www.cs.cmu.edu/~aleven/Papers/2016/Aleven_etal_Handbook2016_AdaptiveLearningTechnologies_Prepub.pdf).
- The same review reports that a machine-learned adaptive policy can change behaviour or time-on-task without necessarily improving domain learning, and argues that next-step hints should be evaluated against a strong, reasonable non-adaptive alternative. [Aleven et al., 2016](https://www.cs.cmu.edu/~aleven/Papers/2016/Aleven_etal_Handbook2016_AdaptiveLearningTechnologies_Prepub.pdf).

### Recommendation / inference for a safe policy

Use a **least-help-first, error-responsive ladder**:

- First ask the learner to identify what is known/unknown or explain the stuck point.
- Then offer a conceptual prompt or worked micro-example; next offer a strategy cue; only later expose a partial step or answer check. Preserve an attempt and explanation before revealing a full solution.
- Adapt to the current error and knowledge estimate, but fade structure after demonstrated success. Do not equate “few hints” with learning: a learner can avoid help and remain wrong.
- Treat rapid repeated hint requests, answer-copy patterns or abandonment as **support signals** (confusion, accessibility, motivation, connectivity), not misconduct proof.

Leading measures should include time to first attempt, hint tier requested, post-hint correction, explanation quality, independent retry success, and abandonment. Lagging measures should include delayed no-hint retention and transfer. A/B or stepped-wedge pilot may compare policies, but randomisation and interpretation need teacher/research governance; optimising for speed alone is not evidence of learning.

## 4. Academic-integrity signals: useful as prompts, not verdicts

### Sourced findings and limitations

- OpenAI’s “New AI classifier for indicating AI-written text” (Jan Hendrik Kirchner, Lama Ahmad, Scott Aaronson and Jan Leike; 31 January 2023) states that its classifier was discontinued on 20 July 2023 because of low accuracy. On an English challenge set it identified only 26% of AI-written text as likely AI-written and falsely labelled human text 9% of the time. OpenAI explicitly says it should not be used as a primary decision tool; it performs worse on non-English text, short text and code, and text can be edited to evade detection. [OpenAI, 2023](https://openai.com/index/new-ai-classifier-for-indicating-ai-written-text/).
- Therefore, a detector score is not authorship proof. Applying the English result to Thai is not validated; the conclusion that Thai detection may be especially unreliable is an **inference** from OpenAI’s explicit non-English limitation, not a Thai performance estimate.
- TEQSA’s *Assessment reform for the age of artificial intelligence* (Tertiary Education Quality and Standards Agency, 23 November 2023) frames the response as assessment redesign that both prepares students for ethical AI use and maintains trustworthy evidence of competence, rather than relying on piecemeal fixes. [TEQSA, 2023](https://www.teqsa.gov.au/guides-resources/resources/corporate-publications/assessment-reform-age-artificial-intelligence).

### Recommendation / inference

Show teachers a **provenance/context panel**, never a “cheating probability”: draft/version history, source/citation changes, unusually abrupt answer changes, hint/solution exposure, and a short oral or in-class explanation. These are prompts for a private, restorative conversation and a re-check of learning—not automatic penalties. Label each signal with what it can and cannot establish, record the teacher’s decision, and provide an appeal/correction path. A legitimate permitted AI interaction must not be counted as misconduct. Keep integrity signals separate from mastery estimates, grades and support grouping.

Suggested integrity leading indicators: percentage of flagged cases reviewed by a teacher; teacher-confirmed rate by signal and language/task type; false-positive rate from blind human review; student appeal rate/outcome; and proportion resolved by additional evidence rather than sanction. A high detector flag rate is **not** a success KPI.

## 5. Teacher, student, safety and privacy requirements

### Sourced findings

- UNICEF’s *Policy Guidance on AI for Children, Version 2.0* (UNICEF, 2021) requires a child-rights approach, inclusion, fairness/non-discrimination, responsible data handling, privacy by design, group-level protection, continuous impact monitoring, safety/security/robustness testing, age-appropriate transparency, accountability and redress. It also calls for infrastructure investment to address the digital divide. [UNICEF, 2021 PDF](https://www.unicef.ch/en/media/5842/download) (the PDF identifies the source as UNICEF, 2021).
- UNESCO’s *Guidance for Generative AI in Education and Research* (UNESCO, 2023) is the relevant global education guidance; its official record is [UNESCO Digital Library, 2023](https://unesdoc.unesco.org/ark:/48223/pf0000386693). UNESCO’s public materials describe a human-centred approach, privacy protection, validation of tools and teacher oversight. Treat the guidance as governance context, not as evidence that any particular vendor model is safe.
- Thailand’s Personal Data Protection Committee (PDPC) is the authoritative place to check current Thai PDPA notifications and guidance. Product deployment should be reviewed against the school’s controller/processor roles and current Thai requirements rather than relying on this note. [Thai PDPC](https://www.pdpc.or.th/).

### Recommendation / inference: minimum safeguards

- **Data minimisation:** use a pseudonymous learner ID; collect only evidence needed for the stated learning purpose. Do not put health, disability, family, disciplinary or other sensitive details into prompts unless an approved, necessary process exists.
- **Purpose and retention:** clearly tell students, caregivers and teachers what is recorded (answers, revisions, hints, model outputs, integrity signals), who can see it, how long it is retained, and how to correct/export/delete it where applicable. Do not reuse learner traces to train a vendor model without a separately reviewed lawful basis and notice.
- **Human control:** teacher approval is required before generated homework or feedback reaches a class; teachers can inspect evidence, edit/reject feedback, override groups, disable hints and resolve flags. No automated high-stakes grade, discipline or access decision.
- **Safety testing:** red-team Thai and local-context outputs for hallucination, stereotyping, harassment, unsafe advice, disclosure of personal data and inappropriate content. Record incidents, surface uncertainty, and escalate to a human. Provide age-appropriate explanations and an appeal route.
- **Equity/accessibility:** do not assume every learner has a personal device, stable broadband, a quiet home or equal Thai-language support. Offer paper/teacher-led/offline equivalents and measure outcomes by access mode, disability accommodation and language without exposing these groups to stigma.
- **Teacher safety/workload:** rate feedback usefulness and correction burden; set queue limits and allow batch review. A system that increases surveillance, unpaid labour or conflict with families is not successful even if click-through improves.

## 6. Draft KPI and measurement framework

All KPIs below are **recommendations**, not sourced benchmarks. Report counts and rates with denominators, confidence intervals where feasible, and disaggregation by school, subject, grade, language, access mode and relevant accommodations. Avoid publishing small-cell subgroup results.

| Layer | Candidate measure | Why it matters / guardrail |
|---|---|---|
| Input/quality | Teacher approval rate; rubric alignment defects per task; proportion of tasks with a novel transfer demand; Thai-language safety/accuracy review pass rate | Measures whether AI generation is usable and valid before student exposure; approval rate alone can reward rubber-stamping. |
| Formative leading | Median feedback latency; teacher edit/reject rate; proportion of feedback tied to rubric evidence; student “understood next step” pulse | Feedback must be actionable and evidence-linked, not merely fluent. |
| Learning-process leading | Attempt before hint; hint tier; post-hint correction; independent retry; revision cycle; abandonment; time on task by access mode | Distinguishes productive help-seeking from answer extraction and detects access or safety problems. |
| Mastery/grouping | Calibration of mastery probabilities; reassessment interval; movement across temporary action groups; teacher override rate; group-size/duration; percentage receiving an independent transfer check | Tests whether grouping is responsive and reversible. Never optimise for stable groups or low movement. |
| Analytical lagging | Rubric dimension score change on parallel tasks; delayed no-hint retention; novel transfer score; explanation/oral-defence score; inter-rater agreement | Directly tests reasoning, not only completion or AI-assisted answer quality. |
| Equity lagging | Learning gain and completion gaps by access mode/language/accommodation; differential false flags; differential teacher overrides; support-group exposure time | Averages can hide harm. Do not use sensitive subgroup membership as a label or feature without governance. |
| Integrity safety | Teacher-confirmed signal rate; false-positive rate from blinded review; appeal rate and resolution; percent of flags resolved with additional evidence; incidents of unauthorised disclosure | The goal is fair review and trustworthy evidence, not more accusations. |
| Student/teacher safety | Reported harmful output; privacy incidents; unsafe-content blocks; teacher correction burden; student sense of agency/trust; opt-out/offline availability | Safety and agency are release-blocking constraints, not secondary engagement metrics. |
| Outcome/implementation | Independent evaluator or teacher agreement; retention after 2–6 weeks; course completion; attendance/engagement; teacher adoption/renewal | Separates short-term clicks from durable learning and sustainable use. |

**Release gates (recommendation):** no launch for a cohort until teacher review is available, a non-digital fallback exists, high-stakes automation is disabled, integrity flags are explicitly non-conclusive, group states are reversible, and safety/privacy incident handling is documented. Pilot with teacher oversight and pre-specified stop criteria; do not claim causal impact from before/after telemetry alone.

## Source index (direct URLs)

1. OECD (2023), *PISA 2022 Assessment and Analytical Framework*, mathematics: <https://www.oecd.org/en/publications/pisa-2022-assessment-and-analytical-framework_dfe0bf9c-en/full-report/component-3.html>.
2. OECD (2023), *PISA 2022 Assessment and Analytical Framework*, creative thinking: <https://www.oecd.org/en/publications/pisa-2022-assessment-and-analytical-framework_dfe0bf9c-en/full-report/component-5.html>.
3. OECD (2005), Janet Looney (project manager/editor), *Formative Assessment: Improving Learning in Secondary Classrooms*: <https://www.oecd.org/en/publications/formative-assessment_9789264007413-en.html>.
4. Rosen & De Meyer (2008), OECD, *Teaching, Learning and Assessment for Adults: Improving Foundation Skills—Case Study: Belgium (Flemish Community)*: <https://web-archive-storage.oecd.org/aemint-web-archive-prod/web-archive/55/5564669b87cb3d000e2d3e40c901668c638719270c9aafecc8c154b1df759995.pdf>.
5. Corbett & Anderson (1995), “Knowledge tracing: Modeling the acquisition of procedural knowledge”: <https://doi.org/10.1007/BF01099821>.
6. Aleven, McLaughlin, Glenn & Koedinger (2016), *Instruction Based on Adaptive Learning Technologies*, Carnegie Mellon University pre-publication copy: <https://www.cs.cmu.edu/~aleven/Papers/2016/Aleven_etal_Handbook2016_AdaptiveLearningTechnologies_Prepub.pdf>.
7. OECD (2023), *PISA 2022 Results (Volume II), Chapter 4: Selecting and grouping students*: <https://www.oecd.org/en/publications/pisa-2022-results-volume-ii_a97db61c-en/full-report/component-11.html>.
8. OpenAI, Kirchner, Ahmad, Aaronson & Leike (31 January 2023), “New AI classifier for indicating AI-written text”: <https://openai.com/index/new-ai-classifier-for-indicating-ai-written-text/>.
9. TEQSA (23 November 2023), *Assessment reform for the age of artificial intelligence*: <https://www.teqsa.gov.au/guides-resources/resources/corporate-publications/assessment-reform-age-artificial-intelligence>.
10. UNICEF (2021), *Policy Guidance on AI for Children, Version 2.0*: <https://www.unicef.ch/en/media/5842/download>.
11. UNESCO (2023), *Guidance for Generative AI in Education and Research*: <https://unesdoc.unesco.org/ark:/48223/pf0000386693>.
12. Thailand Personal Data Protection Committee (PDPC), official portal: <https://www.pdpc.or.th/>.
