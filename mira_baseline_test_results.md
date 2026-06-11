# Mira - Baseline Test Results

## Email Formatting Note

All Gmail outputs are converted from markdown to HTML before sending via a dedicated Convert to HTML Code node. This produces properly formatted emails with styled headers, tables, bullet lists, and a dynamic subject line showing the actual report date. Tested and verified rendering correctly in Gmail.

---


**Project:** Mira: AI-Powered Project Intelligence Assistant
**Company:** Nexora Pvt. Ltd. / ABCDE Ltd. AI Adoption Project
**Tested by:** Nexora TPM
**Platform:** n8n Cloud v2.21.8
**Model:** gpt-4o-mini
**Date:** June 1, 2026

---

## Planner Agent

### T1 - Project Plan (Detailed Input)

**Input:**
Generate a project plan for the AI Adoption Project at ABCDE Ltd. Use the provided project description.

**Output:**
Project Plan: AI Adoption Project for ABCDE Ltd.

Overview
ABCDE Ltd. is launching an AI adoption initiative aimed at enhancing operational efficiency, improving customer experience, and driving data-driven decision-making through the integration of AI tools into its core business functions. The project will leverage machine learning and natural language processing to streamline processes, reduce manual overhead, and provide actionable insights from data.

Phases

Phase 1: Project Initiation
Timeline: Week 1 to Week 2
Key Activities: Define project scope; Form cross-functional team; Initial stakeholder meeting
Key Deliverables: Project charter signed off; Team structure finalized

Phase 2: Current State Analysis
Timeline: Week 3 to Week 4
Key Activities: Assess existing processes and systems; Identify data sources and gaps
Key Deliverables: Current systems audit report; Gap analysis report

Phase 3: Use Case Selection
Timeline: Week 5 to Week 6
Key Activities: Shortlist priority AI use cases (forecasting; route optimization)
Key Deliverables: Final list of prioritised use cases with success criteria

Phase 4: Pilot Design
Timeline: Week 7 to Week 10
Key Activities: Data collection and preparation; Model design and development
Key Deliverables: Working prototype models; Data pipelines established

Phase 5: Pilot Implementation
Timeline: Week 11 to Week 14
Key Activities: Deploy AI models in test environments; Monitor performance
Key Deliverables: Pilot results report; User feedback and performance metrics

Phase 6: Evaluation and Iteration
Timeline: Week 15 to Week 16
Key Activities: Evaluate pilot outcomes; Adjust models as needed
Key Deliverables: Updated models and process refinements

Phase 7: Full Deployment
Timeline: Week 17 to Week 22
Key Activities: Scale AI models across selected departments; Train staff
Key Deliverables: Full production rollout; Training sessions completed

Phase 8: Monitoring and Review
Timeline: Week 23 to Week 24
Key Activities: Continuous performance monitoring; Final project review
Key Deliverables: Project closure report; Recommendations for future scaling

Success Criteria
- Reduce delivery delays by 15%
- Lower operational costs by 10%
- Improve service responsiveness

**Expected keywords present?**
Yes: phases, milestones, timeline, grounded in project description, success criteria with actual targets

**Hallucination detected?**
No. All 8 phases traceable to timeline CSV. Success criteria pulled directly from description. No invented goals, tools, or team structures.

**Tokens used:** 1,382

**Pass/Fail:** PASS

---

### T2 - Project Plan (Vague Input)

**Input:**
Generate a project plan for: "We want to build a chatbot."

**Output:**
I need more information to generate a project plan. Please provide:
- A clear project description including goals and scope
- Approximate timeline or duration
- Key focus areas or use cases

I cannot generate a plan from a one-line prompt as I would have to invent details that may be incorrect.

**Expected keywords present?**
Yes: flags insufficient detail, asks for scope, timeline, and focus areas. No plan generated.

**Hallucination detected?**
No. Correctly refused to generate a plan from a one-line input.

**Tokens used:** 945

**Pass/Fail:** PASS

---

### T9 - Project Plan (Edge Case - Timeline Only, No Details)

**Input:**
Generate a project plan for a 2-week project with no other details.

**Output:**
I need more information to generate a project plan. Please provide:
- A clear project description including goals and scope
- Approximate timeline or duration
- Key focus areas or use cases

I cannot generate a plan from a one-line prompt as I would have to invent details that may be incorrect.

**Expected keywords present?**
Yes: flags insufficient detail, asks for scope, goals, and deliverables. No plan generated.

**Hallucination detected?**
No. Correctly refused to generate a detailed plan from a timeline-only input.

**Tokens used:** 946

**Pass/Fail:** PASS

---

### T11 - Milestone Lookup (Next 2 Weeks)

**Input:**
What milestones are coming up in the next 2 weeks based on the timeline?

**Output:**
Upcoming Milestones

Current project position: Week 10 of 24

1. Phase: Pilot Design
   Timeline: Week 7 to Week 10
   Key Deliverables: Working prototype models; Data pipelines established

2. Phase: Pilot Implementation
   Timeline: Week 11 to Week 14
   Key Deliverables: Pilot results report; User feedback and performance metrics

**Expected keywords present?**
Yes: references actual milestones from timeline CSV, correct week position, current phase and immediately following phase shown.

**Hallucination detected?**
No. Both phases traceable directly to timeline CSV. No invented milestones.

**Prompt iteration note:**
Required 3 prompt iterations to get week calculation correct. Root cause was the model overriding a soft text instruction with its own estimate. Fixed by computing the week number precisely in JavaScript (work weeks from Monday March 30, 2026) and injecting it as a hard system fact the model cannot override. This is documented as a prompt engineering finding for the writeup.

**Tokens used:** 1,116

**Pass/Fail:** PASS

---

## Risk Assessor Agent

*Results to be added*

---

## Status Reporter Agent

*Results to be added*

---

## Stakeholder Updater Agent (Extended)

*Results to be added*

---

## Summary Table

| Test | Agent | Type | Pass/Fail | Hallucination | Tokens |
|------|-------|------|-----------|---------------|--------|
| T1 | Planner | Detailed input | PASS | None | 1,382 |
| T2 | Planner | Vague input | PASS | None | 945 |
| T3 | Risk Assessor | Detailed input | PASS | None | 1,311 |
| T4 | Risk Assessor | Vague input | PASS | None | 868 |
| T5 | Status Reporter | Sprint data | PASS | None | 2,190 |
| T6 | Status Reporter | No data | PASS | None | 811 |
| T7 | Risk Assessor | Top 3 risks | PASS | None | 1,027 |
| T8 | Status Reporter | Blocked/overdue tasks | PASS | None | 2,194 |
| T9 | Planner | Edge case | PASS | None | 946 |
| T10 | Status Reporter | Task count summary | PASS | None | 2,085 |
| T11 | Planner | Milestone lookup | PASS | None | 1,116 |
| T12 | Stakeholder Updater | Sprint 2 email | PASS | None | ~800 |

## Risk Assessor Agent

### T3 - Risk Assessment (Detailed Input)

**Input:**
Generate a risk assessment for the AI Adoption Project at ABCDE Ltd.

**Output:**
Risk Assessment: AI Adoption Project at ABCDE Ltd.

Overview: The risk profile indicates challenges around data management, legacy system integration, and user adoption.

All 10 risks listed: R01 through R10, each with category, risk challenge, impact, and mitigation pulled directly from the CSV.

Summary: Total risks identified: 10. Categories covered: all 10 categories.

**Expected keywords present?**
Yes: all 10 risk IDs present, categorised risks relevant to logistics and AI, impacts and mitigations from CSV.

**Hallucination detected?**
No. Every risk traceable to a specific risk_id in the CSV. No generic or invented risks added.

**Tokens used:** 1,311

**Pass/Fail:** PASS

---

### T4 - Risk Assessment (Vague Input)

**Input:**
Generate a risk assessment for: "New project starting soon."

**Output:**
I need more information to generate a risk assessment. Please provide:
- A project description including goals and scope
- Any known risks or constraints

I cannot generate project-specific risks from a vague prompt as I would have to invent details that may be incorrect.

**Expected keywords present?**
Yes: flags insufficient detail, asks for project description and constraints. No risks invented.

**Hallucination detected?**
No. Correctly refused to generate risks from a vague one-line input.

**Tokens used:** 868

**Pass/Fail:** PASS

---

### T7 - Top 3 Risks

**Input:**
What are the top 3 risks for the ABCDE Ltd project based on the risk data?

**Output:**
Top 3 Risks: ABCDE Ltd Project

1. R05 - Ethical & Legal Concerns: Risks of biased models or data privacy violations. Impact: Legal repercussions and reputational damage.
2. R01 - Data Quality & Availability: Incomplete or unstructured data. Impact: Poor model performance and inaccurate outcomes.
3. R02 - Integration Issues: Difficulty integrating AI with legacy systems. Impact: Delays in deployment and operational disruptions.

**Expected keywords present?**
Yes: references actual risks from risk CSV, correct risk IDs, specific to the project.

**Hallucination detected?**
No. All three risks traceable to CSV with correct risk IDs. Ranking by impact severity is a reasonable model judgement, not an invention.

**Tokens used:** 1,027

**Pass/Fail:** PASS


## Status Reporter Agent

### T5 - Weekly Status Report (Sprint 3 Data)

**Input:**
Generate a weekly status report using the sample task board data for Sprint 3.

**Output:**
Weekly Status Report - Sprint 3. Report Date: June 1 2026.
Tasks In Progress: T007 (Shortlist AI use cases), T025 (Legal review of data usage).
Tasks To Do: T008, T009.
Overdue: T007, T025, T008, T009 all flagged with correct day counts.
Overall Health: Red.

**Expected keywords present?**
Yes: tasks by status, actual task names from CSV, overdue tasks identified, no invented tasks.

**Hallucination detected?**
No. All tasks traceable to Sprint 3 rows in CSV.

**Prompt iteration note:**
Required model switch from gpt-4o-mini to gpt-4o. Mini was reclassifying T007 as Done based on task name rather than reading the status column. gpt-4o follows the status column instruction correctly.

**Tokens used:** 2,190

**Pass/Fail:** PASS

---

### T6 - Weekly Status Report (No Data)

**Input:**
Generate a weekly status report for: "Things are going fine."

**Output:**
I need task board data to generate a status report. Please provide a task board export or sprint data. I cannot generate a status report or fabricate task statuses without actual task data.

**Expected keywords present?**
Yes: states no task data available, refuses to fabricate statuses.

**Hallucination detected?**
No. Correctly refused to generate content from a vague input.

**Observation:**
Model appended "You are trained on data up to October 2023" which is an unprompted model footer. Not a hallucination about project data but noted as noise. Does not affect pass criteria.

**Tokens used:** 811

**Pass/Fail:** PASS

---

### T8 - Blocked and At-Risk Tasks

**Input:**
Which tasks are blocked or at risk of missing their deadline based on the task board?

**Output:**
At-Risk Tasks Report. Report Date: June 1 2026.
8 overdue tasks listed: T006, T007, T008, T009, T010, T011, T012, T025.
All with correct due dates, day counts, and assignees.
Summary: 8 tasks are overdue and need immediate attention.

**Expected keywords present?**
Yes: T024 originally blocked now rescheduled, overdue tasks identified with due dates, correct assignees.

**Hallucination detected?**
No. All 8 tasks traceable to CSV with correct statuses.

**Prompt iteration note:**
Required routing fix. The word "risk" in "at risk" was triggering the Risk Assessor route before the task query route. Fixed by making the Risk Assessor route match specific phrases only (risk assessment, top risks, risk matrix) rather than the single word "risk".

**Tokens used:** 2,194

**Pass/Fail:** PASS

---

### T10 - Task Count Summary

**Input:**
Summarize the current project status: how many tasks are done, in progress, and to do?

**Output:**
Project Status Summary. Report Date: June 1 2026.
Done: 5, In Progress: 3, To Do: 17, Total Tasks: 25.
In Progress: T006, T007, T025.
Overdue Tasks: 8 (all listed correctly).

**Expected keywords present?**
Yes: correct counts matching actual task board data exactly.

**Hallucination detected?**
No. All counts verified against CSV before sending to model.

**Prompt iteration note:**
Required injecting pre-verified counts as hard facts. Model was miscounting CSV rows independently, returning wrong totals. Python now counts precisely and injects Done, In Progress, To Do, Total, and Overdue as system-calculated values the model cannot override. Same pattern used for week number in Planner agent.

**Observation:**
Prompt instruction "Use the VERIFIED COUNTS from the input exactly" leaked into the visible output. Cosmetic issue only, does not affect accuracy. Will clean up in final prompt.

**Tokens used:** 2,085 (estimated)

**Pass/Fail:** PASS


## Stakeholder Updater Agent (Extended Capability)

### T12 - Stakeholder Update Email (Sprint 2)

**Input:**
Generate a stakeholder update email summarising Sprint 2 progress.

**Output:**
Subject: Sprint 2 Update: Progress and Focus Areas

Dear Leadership,

Sprint 2 is currently facing challenges, with overdue tasks impacting our momentum. However, we have made noteworthy progress in key areas.

Progress This Sprint: Successfully audited existing data infrastructure and mapped current logistics workflows, providing vital insights into operational efficiencies.

Work in Flight: Actively identifying data gaps and quality issues, though this task is currently overdue. Risk being addressed promptly.

Looking Ahead: Upcoming sprint focus will shift toward mitigating identified data gaps and enhancing overall data quality.

Regards, Nexora Project Team

**Expected keywords present?**
Yes: references Sprint 2 tasks only, professional tone, outcome-focused language, risk acknowledged appropriately, no Sprint 3+ tasks included.

**Hallucination detected?**
No. All content traceable to Sprint 2 task data. No invented outcomes or metrics.

**Style observation:**
Email opens with the challenge rather than achievements. For a stronger executive tone, leading with outcomes first and risks second would be preferable. Does not affect pass criteria but noted for future prompt refinement.

**Tokens used:** estimated ~800

**Pass/Fail:** PASS


## Extended Tests - Stakeholder Updater

### T13 - Stakeholder Update (Negative - No Sprint Reference)

**Input:**
Send a stakeholder update to the team.

**Output:**
I need a sprint reference to generate a stakeholder update. Please specify which sprint you would like the update for.

**Email drafted:** No. Correctly skipped.

**Expected keywords present?**
Yes: refusal message, asks for sprint reference, no content invented.

**Hallucination detected?**
No. Correctly refused without a sprint reference.

**Pass/Fail:** PASS

---

### T14 - Stakeholder Update (Positive - Sprint 3)

**Input:**
Generate a stakeholder update email summarising Sprint 3 progress.

**Output:**
Dear Leadership, We are actively progressing on key initiatives within Sprint 3, which lays the groundwork for our AI-driven strategy.

Progress This Sprint: Significant strides in identifying AI use cases and conducting a thorough legal review of data usage.

Work in Flight: Finalising shortlist of AI use cases and addressing legal considerations. All tasks slightly behind schedule acknowledged professionally.

Looking Ahead: Focus shifts to defining success criteria and completing vendor evaluation.

**Email drafted:** Yes. Sent to yeoldstudy@gmail.com via Gmail node as formatted HTML email with dynamic subject line.

**Expected keywords present?**
Yes: Sprint 3 tasks referenced, positive opening, risks acknowledged after progress, professional tone, no Sprint 2 tasks included.

**Hallucination detected?**
No. All content traceable to Sprint 3 task data.

**Observation:**
n8n free tier appends "This email was sent automatically with n8n" footer to all Gmail sends. Platform limitation, not a Mira issue. Note in writeup.

**Pass/Fail:** PASS


## Final Summary Table (All 14 Tests)

| Test | Agent | Type | Pass/Fail | Hallucination | Email |
|------|-------|------|-----------|---------------|-------|
| T1 | Planner | Detailed plan | PASS | None | N/A |
| T2 | Planner | Vague input | PASS | None | N/A |
| T3 | Risk Assessor | Detailed assessment | PASS | None | N/A |
| T4 | Risk Assessor | Vague input | PASS | None | N/A |
| T5 | Status Reporter | Sprint 3 report | PASS | None | Sent |
| T6 | Status Reporter | No data | PASS | None | Skipped |
| T7 | Risk Assessor | Top 3 risks | PASS | None | N/A |
| T8 | Status Reporter | Blocked/overdue | PASS | None | Sent |
| T9 | Planner | Edge case | PASS | None | N/A |
| T10 | Status Reporter | Task count summary | PASS | None | Sent |
| T11 | Planner | Milestone lookup | PASS | None | N/A |
| T12 | Stakeholder Updater | Sprint 2 email | PASS | None | Sent |
| T13 | Stakeholder Updater | No sprint reference | PASS | None | Skipped |
| T14 | Stakeholder Updater | Sprint 3 email | PASS | None | Sent |

**Total: 14 tests. 14 pass. 0 fail. 0 hallucinations.**
