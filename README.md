# Mira: AI-Powered Project Intelligence Assistant

**Built by Nexora Pvt. Ltd. for the ABCDE Ltd. AI Adoption Project**

Platform: n8n Cloud | Model: gpt-4o | Trigger: Telegram | Observability: Langfuse

---

## What is Mira?

Mira is an AI-powered project intelligence assistant that automates project planning, risk assessment, status reporting, and stakeholder communications for PMs and TPMs at Nexora Pvt. Ltd. Built as a capstone project for the Applied Agentic AI for PMs/TPMs course.

You send Mira a message on Telegram. Mira reads it, understands what you need, generates a structured data-grounded response using actual project files, and sends it back in seconds. Combined queries work too: ask for a plan and risk assessment in one message and get both back together.

---

## What Mira Can Do

| Capability | Example Query | Output |
|---|---|---|
| Project Plan Generation | Generate a project plan for the AI Adoption Project at ABCDE Ltd. | All 8 phases, activities, deliverables, and success criteria |
| Risk Assessment | Generate a risk assessment for the AI Adoption Project | All 10 risks with IDs, categories, impacts, and mitigations |
| Weekly Status Report | Give me a status report for Sprint 3 | Task counts, in-progress table, overdue alerts |
| Milestone Tracking | What milestones are coming up in the next 2 weeks? | Current phase and next phase with deliverables |
| Stakeholder Update | Generate a stakeholder update email for Sprint 2 | Executive email sent automatically to configured address |
| Combined Queries | Show me the project plan, risks, and Sprint 3 status | All three sections in one structured response |
| At-Risk Tasks | Which tasks are blocked or overdue? | Full overdue task table with assignees and days overdue |
| Task Summary | How many tasks are done, in progress, and to do? | Verified task counts with in-progress and overdue breakdowns |

---

## Architecture (v8 Final)

```
Telegram Trigger
     |
     v
Mira - OpenAI (gpt-4o, all project data in system prompt)
     |
     v
If Email Needed
     |                    |
     v                    v
Convert to HTML      Final Output
     |                    |
     v                    v
Gmail Send           Langfuse Trace
(HTML formatted,          |
 dynamic subject)         v
                 Telegram Send Response
```

**Key design decisions:**
- Single OpenAI call with all data in system prompt eliminates hallucination risk and parallel merge problems
- Native n8n OpenAI node over HTTP Request resolves chatInput expressions reliably
- gpt-4o selected because gpt-4o-mini miscategorised task statuses by reading task names rather than the status column
- Telegram trigger makes Mira accessible from any device without logging into n8n
- One Code node pre-calculates task counts dynamically from CSV so the model always has verified numbers

---

## Repository Structure

| File | Description |
|---|---|
| `mira_full_workflow_v8.json` | Main n8n workflow. Import this to deploy Mira. 8 nodes. |
| `mira_full_workflow_v6.json` | Previous stable version with Switch node routing. All 14 baseline tests passing. |
| `sample_task_board_updated.csv` | Task board with unblocked dates. T024 rescheduled to June 2026. |
| `project_timeline.csv` | Original 8-phase project timeline CSV. |
| `project_risks.csv` | Original 10-risk matrix CSV. |
| `Mira_Architecture_Writeup.docx` | Full architecture writeup with design decisions, testing findings, and n8n integration challenges. |
| `Mira_Assignment_QA.docx` | Assignment Q1 to Q4 responses. |
| `mira_baseline_test_results.md` | All 14 baseline test results plus T13 and T14. |
| `screenshots/` | n8n canvas, Mira in action, Langfuse traces, Telegram demo. |

---

## Setup

### Prerequisites
- n8n Cloud account (Starter plan or above)
- OpenAI API key with gpt-4o access
- Telegram Bot token (create via BotFather)
- Gmail account with OAuth2 credentials in n8n
- Langfuse account (free tier, US region)

### Step 1: Import the Workflow
In n8n, go to the menu and select Import from file. Import `mira_full_workflow_v8.json`.

### Step 2: Connect Credentials

| Node | Credential Type | What to Connect |
|---|---|---|
| Mira - OpenAI | OpenAI API | Your OpenAI API key |
| Telegram Trigger | Telegram API | Your Bot token from BotFather |
| Gmail - Send Report | Gmail OAuth2 | Your Gmail account |
| Telegram - Send Response | Telegram API | Same Bot token as Trigger |
| Langfuse - Mira Trace | None (hardcoded) | Update Base64 string with your own keys before pushing to GitHub |

### Step 3: Update Langfuse Credentials
Before pushing to GitHub, replace the Langfuse Authorization header value:
```
Base64 encode: your_public_key:your_secret_key
Header value: Basic [encoded_string]
```

### Step 4: Activate and Test
Click Publish to activate. Open Telegram and send:
```
Generate a project plan for the AI Adoption Project at ABCDE Ltd.
```
Mira should respond within 10 to 15 seconds.

---

## Baseline Test Results

All 14 baseline tests pass. Zero hallucinations detected.

| Test | Input | Result |
|---|---|---|
| T1 | Generate a project plan for the AI Adoption Project at ABCDE Ltd. | PASS |
| T2 | Generate a project plan for: We want to build a chatbot. | PASS |
| T3 | Generate a risk assessment for the AI Adoption Project at ABCDE Ltd. | PASS |
| T4 | Generate a risk assessment for: New project starting soon. | PASS |
| T5 | Generate a weekly status report using the sample task board data for Sprint 3. | PASS |
| T6 | Generate a weekly status report for: Things are going fine. | PASS |
| T7 | What are the top 3 risks for the ABCDE Ltd project based on the risk data? | PASS |
| T8 | Which tasks are blocked or at risk of missing their deadline based on the task board? | PASS |
| T9 | Generate a project plan for a 2-week project with no other details. | PASS |
| T10 | Summarize the current project status: how many tasks are done, in progress, and to do? | PASS |
| T11 | What milestones are coming up in the next 2 weeks based on the timeline? | PASS |
| T12 | Generate a stakeholder update email summarising Sprint 2 progress. | PASS |
| T13 | Send a stakeholder update to the team. (no sprint specified) | PASS |
| T14 | Generate a stakeholder update email summarising Sprint 3 progress. | PASS |

---

## Known Limitations

- **Telegram character limit:** Responses over 4,096 characters are truncated. Full report sent to Gmail automatically.
- **n8n free tier watermark:** n8n appends a promotional footer to messages on the Starter plan.
- **Langfuse credentials hardcoded:** Replace with environment variables before production deployment.
- **Static project data:** All project data is embedded in the system prompt. Update the workflow JSON to change project data.
- **Single project scope:** Mira is configured for ABCDE Ltd. only.
- **Token cost:** Approximately $0.038 to $0.057 per query due to large system prompt. RAG will reduce this significantly.

---

## Version History

| Version | Key Change | Status |
|---|---|---|
| v1 to v3 | Initial Planner agent build and credential debugging | Archived |
| v4 to v5 | Risk Assessor and Status Reporter added | Archived |
| v6 | Complete working system with Switch node routing. All 14 baseline tests passing. | Stable reference |
| v7 | Telegram trigger experiment with parallel fan-out. Merge deadlock. | Archived |
| v8 | Final version. Telegram, native OpenAI node, single call, combined queries working. | Current |

---

## Course Context

Built as the Mira capstone for the Applied Agentic AI for PMs/TPMs course.

| Component | Implementation |
|---|---|
| Multi-Agent Design | Single intelligent agent with 6 distinct capabilities |
| Orchestration Pattern | Single-agent with embedded data (evolved from Router/Switch after platform testing) |
| Prompt Engineering | 5 documented findings in architecture writeup |
| Data Grounding | All outputs grounded in project CSV files. Zero hallucinations across 14 tests. |
| Evaluation and Observability | Langfuse (US region) connected. Single trace per interaction. |
| Extended Capability | Stakeholder Update Generator with automatic Gmail delivery |
