# Hubblehox Objection Management System (OMS)
## Comprehensive Feature List & User Manual

**Product:** Hubblehox OMS — a government-grade Examination Objection & Result Management System
**Scope:** End-to-end objection lifecycle for high-stakes computer-based tests (CBT), from answer-key challenge through moderation, normalization, scorecard publication, and appeals.
**Document version:** 1.0
**Audience:** Exam authorities, subject experts, moderation committees, candidates, and system administrators.

---

## Table of Contents

1. [What the product does](#1-what-the-product-does)
2. [Roles & access (RBAC)](#2-roles--access-rbac)
3. [The objection-to-result pipeline](#3-the-objection-to-result-pipeline)
4. [Getting started — login](#4-getting-started--login)
5. [Comprehensive feature list](#5-comprehensive-feature-list)
6. [Candidate user guide](#6-candidate-user-guide)
7. [Exam Authority / Administrator guide](#7-exam-authority--administrator-guide)
8. [Subject Expert (SME) guide](#8-subject-expert-sme-guide)
9. [Moderation Committee guide](#9-moderation-committee-guide)
10. [Normalization methods reference](#10-normalization-methods-reference)
11. [Reports catalogue](#11-reports-catalogue)
12. [Data import guide](#12-data-import-guide)
13. [Glossary](#13-glossary)
14. [FAQ & troubleshooting](#14-faq--troubleshooting)
15. [Appendix — demo credentials & technical notes](#15-appendix--demo-credentials--technical-notes)

---

## 1. What the product does

When a high-stakes exam runs across many shifts for hundreds of thousands of candidates, candidates can formally challenge ("object to") the provisional answer key. Hubblehox OMS manages that entire process so that every ruling is **defensible, refundable, versioned, and auditable**:

- Candidates raise objections (with evidence and a fee) against specific questions.
- Objections **auto-route** to the Subject Expert who owns the relevant subject.
- The **question is the unit of review** — one expert ruling covers every candidate who objected to that question, and any key change is re-applied to *all* candidates, not just the objectors.
- A **Moderation Committee** sets the marking/award policy for every accepted question (maker–checker control).
- Raw scores are **normalized across shifts** using a configurable method, then published as **category-wise scorecards**.
- Candidates may **appeal** a decision; fees on upheld objections are **refunded**.
- Every action is logged to a built-in database for audit and reporting.

The system manages **multiple exams** simultaneously (e.g., a live "PCM" exam and a "PCB" exam still inside its objection window).

---

## 2. Roles & access (RBAC)

Access is module-driven: each role sees only the screens it is permitted to use. Empty navigation groups are hidden automatically.

| Role | Who it represents | Primary responsibilities |
|---|---|---|
| **Candidate** | Exam taker | View paper, raise objections, track status, view scorecard, file grievances |
| **Exam Authority / Administrator** | Exam Controller & System Admin | Configure windows & fees, manage objections, routing, normalization, publishing, reports, imports, users, database |
| **Subject Expert (SME)** | Per-subject reviewer | Review objections for their subject and decide accept / reject / clarify |
| **Moderation Committee** | Result authority panel | Set the award policy for every SME-accepted question |

> **Note on the two admin profiles:** *Exam Controller* and *System Admin* are two labels on the **same** administrator role with identical full-module access. They exist to mirror real org structure, not to gate different permissions.

Subject ownership drives routing:

| Subject | Owning Subject Expert |
|---|---|
| Physics | Dr. R. Iyer |
| Chemistry | Dr. M. Khan |
| Mathematics | Prof. S. Bose |

---

## 3. The objection-to-result pipeline

```
Candidate raises objection (+ evidence + fee)
        │  auto-routed to the subject's SME
        ▼
SME review  ──►  Accept (re-key OR drop) │ Reject │ Clarify
        │           one ruling per question → applies to all objectors
        ▼
Moderation Committee  ──►  Award policy (Re-key / Award all / Attempted only)
        │           maker (SME) → checker (Moderation) finalises the key (v2)
        ▼
Normalization  ──►  cross-shift equating (selectable method)
        │
        ▼
Scorecard publishing  ──►  category-wise cutoffs, QR-verified scorecards
        │
        ▼
Appeals / grievances (optional)  +  fee refunds on upheld objections
```

Each gate is **locked** until the previous one is complete. The **Pipeline Control Tower** (on the admin dashboard) shows exactly which gate is blocking publication and links straight to it.

---

## 4. Getting started — login

1. Open the application. The login screen is a split panel: branding on the left, sign-in on the right.
2. Sign in one of two ways:
   - **Login ID + password** — enter a role login ID and the demo password.
   - **Quick-pick profile** — click any pre-seeded profile to sign in instantly.
3. After login you land on that role's default screen. Your name and role appear in the top-right chip; **sign out** from the chip.

> Demo password and login IDs are listed in the [Appendix](#15-appendix--demo-credentials--technical-notes).

You can switch roles by signing out and selecting a different profile — there is no in-app role switch, by design.

---

## 5. Comprehensive feature list

### 5.1 Platform-wide
- **Multi-exam management** — independent pipelines per exam, each with its own window, fee, shifts, and status.
- **Role-based access control** — module-driven navigation; screens hidden when not permitted.
- **In-browser SQLite store** — a real relational database (questions, answers, objections, moderation, shift stats, scores, scorecards, audit) with graceful in-memory fallback.
- **Audit log** — every system, candidate, SME, and admin action is recorded with timestamp, actor, action, and detail.
- **Global search** (staff) — search any roll number, objection ID, or Q-number from the top bar to open a 360° view.
- **Notifications** — top-bar bell with unread badge; event-driven, role-scoped alerts.
- **SLA timers** — live countdowns to the objection-window close and SME review deadlines, with breach highlighting.
- **Accessibility** — skip-to-content link, ARIA live regions, labelled controls, and a reduced-motion mode.
- **Responsive layout** — adapts to mobile and desktop.

### 5.2 Candidate
- Personal dashboard with exam and window status.
- 3-step question paper: **cover → instructions/consent → answer grid**, with section tabs and card/focus views.
- **Raise objection** modal: reason, explanation, supporting reference, document/image upload, and URL.
- **Objection basket** + **review & pay** (flat or per-question fee).
- **Objection status tracker** with full lifecycle and per-objection SME remarks.
- **File a grievance** on any decided objection.
- **My Scorecard**: raw, normalized, percentile, category cutoff, qualified status, QR verification, download/print.
- **Fee refund** notice when objections are upheld.

### 5.3 Exam Authority / Administrator
- **Objection dashboard** with Pipeline Control Tower, live SLA card, and fee/refund KPIs.
- **Window & Fee** configuration (start/end date & time; flat or per-question fee).
- **Objection Management**: per-exam stat cards → drill-in with subject/SME breakdowns, **objection heatmap**, category totals, **By-objection / By-question** views, working filters, **bulk dismiss/reassign**, and one-click CSV report.
- **SME Assignment & Routing**: auto-routing map, manual-assignment queue (only for unowned subjects), and reassignment table.
- **Appeals & Grievances** queue with uphold/dismiss.
- **Key Revisions**: two-stage (provisional → final) answer-key log with maker–checker trail.
- **Normalization**: selectable method, reference batch, live preview, and run engine.
- **Scorecard Publishing**: live-window scheduling, generation, **category-wise results**, and version history.
- **Reports**: 12 live CSV exports.
- **Data Import**: CSV ingestion of shift statistics, answer key, and candidate responses.
- **User Management**: staff directory and create-user (with subject binding for SMEs and module assignment).
- **Database** explorer with a SQL console and audit log.

### 5.4 Subject Expert (SME)
- **Review desk** that groups objections **by question** (one review per question), with objection counts, reasons raised, and an SLA pill.
- **Review modal** showing the question, options, current key, candidate response, the full **multi-objector evidence** cluster, and the decision controls.
- **Decision options**: Accept (re-key to a corrected option, or drop the question), Reject, or Clarify — with mandatory remarks.
- **Ruling propagation**: one decision applies to every objector on the question.
- **Decision history** audit trail.

### 5.5 Moderation Committee
- **Award moderation** queue grouped by question, with an **impact simulation (what-if)** showing affected candidates and estimated qualified-status flips.
- **Award policy** per accepted question: Re-evaluate (corrected key), Award to all candidates, or Award to attempted only.
- **One policy per question** covering all objectors; finalises the key (the "checker" in maker–checker).
- **Moderation history** audit trail.

---

## 6. Candidate user guide

### 6.1 View your question paper
1. Open **Question Paper**.
2. Read the **cover** and click through to **instructions**; accept the consent to proceed.
3. The **answer grid** opens. Use the **section tabs** (e.g., Physics / Chemistry / Mathematics) to move between sections, and toggle **card** or **focus** view.

### 6.2 Raise an objection
1. On any question, click **Raise objection**.
2. Complete the form:
   - **Reason** — choose the objection type (e.g., *Answer Key is Incorrect*, *Multiple Correct Answers*, *No Correct Answer*).
   - **Explanation** *(required)* — state precisely why the question/key is wrong.
   - **Supporting reference** *(required)* — e.g., a textbook citation.
   - **Document / URL** *(optional)* — attach evidence.
3. Click **Add to basket**. Repeat for other questions.
4. Open **Review & submit**, confirm the items and **fee**, and complete payment.
5. Your objections are submitted and **auto-routed** to the subject expert. You'll see a receipt ID.

> Evidence you attach travels with the objection and is shown to the expert during review — strong references improve your odds.

### 6.3 Track status & remarks
- Open **Objection Status** to see the **lifecycle** (Submitted → SME review → Moderation → Key finalised → Normalization → Scorecard) and each objection's current state plus the **SME remark** once decided.

### 6.4 File a grievance (appeal)
- On any **decided** objection (accepted or rejected), click **File grievance**, state your grounds, and submit. Track the appeal status (open / upheld / dismissed) on the same screen.

### 6.5 View your scorecard
- Open **My Scorecard**. Before results it shows a pending/scheduled state. Once live, it displays **raw, normalized, percentile, category cutoff**, and **qualified** status, with QR verification and download/print.
- If any of your objections were upheld, a **fee-refund** notice appears.

### 6.6 Notifications
- The **bell** (top bar) alerts you when an objection is decided or when your scorecard is published.

---

## 7. Exam Authority / Administrator guide

### 7.1 Dashboard & control tower
- The **Pipeline Control Tower** lists each gate (objection review → moderation → normalization → publish), its status, who owns it, and a deep link to act. The "next blocker" is called out.
- The **SLA & deadlines** card shows live countdowns and how many reviews are breaching SLA.
- KPIs show total/pending/accepted objections and **net fee retained** (collected − refunded).

### 7.2 Configure the window & fee
1. Open **Window & Fee** and select an exam.
2. Set the objection **window** start/end date & time.
3. Choose a **fee model** — *flat* (one-time per submission) or *per-question* — and the amount.

### 7.3 Manage objections
1. Open **Objection Management** and pick an exam (cards show total / resolved / pending).
2. Inside an exam:
   - **KPIs**: total objections, distinct questions, resolved, pending.
   - **By subject** and **By SME** breakdown cards (click to filter).
   - **Objection heatmap**: a 150-cell grid shaded by objection volume; the hottest questions are flagged.
   - **Objections by category**: total per objection type, with a **Download report (CSV)** button.
   - **View toggle**: *By objection* (register) or *By question* (aggregated). The by-question table shows count, type breakdown, candidates, SME, and ruling, with a **360** drill-in.
   - **Filters**: subject, status, and SME (combine freely; "Clear" resets).
   - **Bulk actions**: select rows to **Dismiss** (admin-level rejection) or **Reassign** to an SME.

### 7.4 Routing & reassignment
- Open **SME Assignment**. New objections auto-route to the owning expert; this screen surfaces **only** objections needing manual assignment (subjects with no active expert) and lets you **reassign** any active objection. The **SME workload** panel shows pending/total per expert.

### 7.5 Appeals & grievances
- Open **Appeals & Grievances** to review escalations. **Uphold** (ruling changes) or **Dismiss** (ruling stands); the candidate is notified and the action is logged.

### 7.6 Key revisions
- Open **Key Revisions** to see the **two-stage** answer-key trail: provisional (v1) → revised (v2), with **maker** (SME) and **checker** (Moderation Committee) and the current stage. No single person finalises a key alone.

### 7.7 Normalization
1. Open **Normalization** and select an exam (locked until all objections are resolved and every accepted question is moderated).
2. Choose a **method** (see [Section 10](#10-normalization-methods-reference)); for Equi-Percentile pick a **reference batch**; for Custom, enter and validate a formula.
3. Review the **live preview** for the sample candidate, then **Run normalization**.

### 7.8 Publish scorecards
1. Open **Scorecard Publishing** (locked until normalization is done).
2. Set the scorecard **live window**, then **Publish**.
3. Review the **category-wise results** table (cutoff, candidates, qualified per category). Use **Regenerate** for post-result corrections (version history is retained).

### 7.9 Reports, import, users, database
- **Reports** — download any of the 12 reports as live CSV (see [Section 11](#11-reports-catalogue)).
- **Data Import** — ingest CSV (see [Section 12](#12-data-import-guide)).
- **User Management** — view staff and **create users**; creating an SME binds a subject (which drives routing) and the new user can log in.
- **Database** — browse tables, run read-only SQL, and inspect the audit log.

### 7.10 Global search
- From the top bar, type a **roll number**, **objection ID**, or **Q-number** and press Enter to open the relevant **Candidate 360** or **Question 360** view.

---

## 8. Subject Expert (SME) guide

### 8.1 Your review desk
- Open **Review Queue**. Objections are grouped **by question** — one row per question with its **objection count**, **reasons raised**, an **SLA** countdown, and status. KPIs show questions on desk, awaiting decision, and decided.

### 8.2 Review a question
1. Click **Review** on a question.
2. The modal shows the **question, options, and current key**, a note if **multiple candidates objected**, and the **evidence** every objector submitted (explanation, reference, link, file).
3. Choose a **decision**:
   - **Accept** → then **Accept as**: select the corrected option (**re-key**) or **Drop question — all options wrong**.
   - **Reject** → the key stands.
   - **Clarification** → send back for clarification.
4. Record your **remarks** (rationale) and submit.

> **One ruling covers everyone.** Your decision applies to all candidates who objected to that question; an accepted re-key updates the master key and is referred to the Moderation Committee. The toast confirms how many objections were affected.

### 8.3 Decision history
- **Decision History** is your auditable trail of every accept/reject/clarify with remarks and timestamps.

---

## 9. Moderation Committee guide

### 9.1 Award moderation
- Open **Award Moderation**. Every **SME-accepted question** appears as a single card (with its objection count). KPIs track accepted questions, pending policy, and dropped items.

### 9.2 Impact simulation (what-if)
- Before deciding, review the **Impact simulation** panel: per accepted question, the **estimated candidates affected** and **qualified-status flips**, plus totals. Use it to gauge the cohort-level effect of each ruling.

### 9.3 Set the award policy
- For each accepted question choose:
  - **Re-evaluate (corrected key)** — credit candidates who selected the corrected option.
  - **Award to all candidates** — everyone receives the marks (typical when a question is dropped).
  - **Award to attempted only** — only candidates who answered receive marks.
- One policy applies to all objectors on the question and **finalises the key (v2)**. Once every accepted question is moderated, **Normalization unlocks** for the Exam Authority.

### 9.4 Moderation history
- **Moderation History** records every award-policy decision for audit.

---

## 10. Normalization methods reference

Normalization equates raw scores across shifts that may differ in difficulty. The Exam Authority selects one method per exam.

| Method | Use case | Formula (per candidate) |
|---|---|---|
| **Multi-Shift Z-Score** *(recommended)* | One exam across multiple shifts | `normalized = mean_all + sd_all × ((X − mean_shift) / sd_shift)` |
| **Single-Batch Percentile** | One paper, one batch | `percentile = (count_at_or_below / n) × 100` (mid-rank, tie-fair) |
| **Equi-Percentile Equating** | Map onto a chosen reference batch's scale | `equated = referenceScoreAtPercentile(percentile_in_own_batch)` (interpolated) |
| **Custom formula** | Author-defined equating | Your expression over the available variables |

**Key safeguards (Z-Score):** each candidate is divided by **their own** shift's standard deviation; population SD is used throughout; a `sd_shift = 0` guard prevents division by zero; results are clamped to the legal mark range.

**Custom-formula variables:** `X`, `mean_shift`, `sd_shift`, `mean_all`, `sd_all`, `n`, `z`, `percentile`, `maxMarks`.
**Allowed functions:** `sqrt, abs, min, max, pow, log, exp, round, floor, ceil` (plus `PI`, `E`).
The evaluator is sandboxed — any name outside the allowed variables/functions (e.g., `window`, `fetch`) is rejected before execution. Use **Validate & preview** to test before running.

> **Fairness note:** the Z-Score output depends only on a candidate's position **within their own shift**, which is fair only if shifts are equal in ability (random allocation). State this assumption when signing off.

---

## 11. Reports catalogue

All reports download as **live CSV** generated from current data. Some unlock at later pipeline stages.

| Report | Contents | Availability |
|---|---|---|
| **Objection Summary** | Every objection by question, subject, candidate, status, ruling, fee, refund | Live exam |
| **Question-wise Objection Analysis** | Per question: count, candidates, type breakdown, category totals row | Live exam |
| **SME Decision Audit** | Every accept/reject/clarify with SME, rationale, timestamp | Live exam |
| **SME Performance & SLA** | Per expert: assigned, accept rate, turnaround, overturned-at-moderation | Live exam |
| **Answer-Key Revision Log** | Provisional → revised key, maker, checker, stage | Live exam |
| **Fee & Refund Reconciliation** | Collected vs refunded vs net, plus per-objection ledger | Live exam |
| **Impact Analysis (pre-publication)** | Per accepted question: candidates affected, estimated flips | Live exam |
| **Shift-wise Statistics** | Mean, SD, top score per shift | Live exam |
| **Normalization Audit** | Method, per-shift parameters, pooled mean/SD, equal-ability assumption | After normalization |
| **Category-wise Result** | Cutoff, candidates, qualified per category | After publish |
| **Audit-Log Export** | Complete tamper-evident action log | Live exam |
| **Candidate Master Data** | Registrations, categories, shift allocation | Always |

---

## 12. Data import guide

Open **Data Import** (Administration). Three CSV feeds are supported. Paste the CSV (or **Load sample**), then **Import CSV**; a preview of the first rows confirms the load.

| Feed | Required columns | Effect |
|---|---|---|
| **Shift statistics** | `shift, candidates, mean, sd, top` | Feeds the normalization engine directly |
| **Provisional answer key** | `question, key` | Loads as the provisional (v1) key candidates object against |
| **Candidate responses** | `roll, question, answer` | Updates the response sheet used for scoring |

Imports write to the live store — inspect them on the **Database** screen, and see the effect immediately on **Normalization** and **Reports**. The CSV parser handles quoted fields and commas within quotes.

---

## 13. Glossary

- **Objection** — a candidate's formal challenge to a question or its answer key.
- **Provisional key (v1)** — the answer key released for the objection window.
- **Final key (v2)** — the key after accepted corrections are confirmed by moderation.
- **Re-key** — changing a question's correct option.
- **Drop** — invalidating a question because no option is defensible.
- **Award policy** — how marks for an accepted/dropped question are credited (re-key / all / attempted).
- **Maker–checker** — the SME proposes a key change (maker); the Moderation Committee confirms it (checker).
- **Normalization** — equating raw scores across shifts onto a common scale.
- **Percentile cutoff** — the category threshold a candidate must meet to qualify.
- **SLA** — service-level deadline (objection window close, SME review turnaround).
- **Grievance / appeal** — escalation of a decided objection for final review.
- **360 view** — a consolidated screen for a single question or candidate.

---

## 14. FAQ & troubleshooting

**Why are some objections "unassigned"?**
Only when no active expert owns that subject. Assign one manually, or create an SME for the subject in User Management.

**A question has many objections — do I review each one?**
No. The SME reviews the **question once**; the ruling applies to all objectors and the moderation policy covers them too.

**Normalization is locked — why?**
All objections must be resolved **and** every accepted question must be moderated. The Control Tower shows the exact blocker.

**Does changing a key affect only the objectors?**
No. A re-key/drop is re-applied to **all** candidates during re-evaluation.

**When is a fee refunded?**
When the objection is **upheld** (accepted). The candidate sees a refund notice; reconciliation appears in the Fee & Refund report.

**Reports just download empty/short files?**
Reports reflect current data. Stage-gated reports (Normalization Audit, Category-wise Result) populate only after those stages complete.

**The countdown shows "breached."**
That SLA deadline has passed; prioritise those reviews. Breach counts surface on the dashboard SLA card and SME desk.

---

## 15. Appendix — demo credentials & technical notes

### Demo sign-in
- **Password (all profiles):** `demo@1234`
- Or use the **quick-pick profiles** on the login screen.

| Role | Sample profile | Login ID |
|---|---|---|
| Candidate | Aarav Sharma (OBC, Shift 3) | `PCM2026100482` |
| Administrator | Meera Krishnan — Exam Controller | *(quick-pick)* |
| Administrator | Control Room — System Admin | *(quick-pick)* |
| Subject Expert | Dr. R. Iyer — Physics | `SME-PHY-04` |
| Subject Expert | Prof. S. Bose — Mathematics | `SME-MAT-07` |
| Subject Expert | Dr. M. Khan — Chemistry | `SME-CHE-09` |
| Moderation Committee | Sunita Rao | `MOD-RES-01` |

### Scoring model (demo exam "PCM")
- 150 questions across 3 sections (50 each); 2 marks per question; 0.5 negative marking; **300** maximum.
- Sample candidate **Aarav Sharma**: baseline raw **265 → 91.55 percentile**, below the OBC cutoff (92) — **not qualified**. After his accepted objections flip the key, raw **270 → 93.32 percentile** — **qualified**. This demonstrates how the pipeline changes outcomes end-to-end.

### Technical notes
- The product is a **single, self-contained application** with an in-browser **SQLite** store (loaded at runtime) and a graceful in-memory fallback.
- All actions are written to an **audit** table and surfaced in the Database screen and the Audit-Log report.
- This is a functional **prototype**. A production deployment would add a server-side database and persistence, authentication/SSO, durable file storage for evidence, real email/SMS notification delivery, and load testing for full candidate volumes.

---

*Hubblehox Objection Management System — User Manual v1.0. For the most current in-app behaviour, follow the on-screen labels and tooltips, which always reflect the live build.*
