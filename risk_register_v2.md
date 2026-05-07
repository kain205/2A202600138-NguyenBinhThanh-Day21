# Risk Register v2 — Synapsy
**Scoring:** Likelihood (1–5) × Impact (1–5 by runway loss)
**Status:** Updated for MinerU Pipeline (Native Vision deprecated)

---

## 🔥 THE KILL ZONE (Top 5 Priority)
*Mitigations for these must be implemented this week. Cost <$500/mo.*

### RISK 1: MinerU Free Tier Extraction Halt
**Type:** Vendor
**If:** MinerU's free tier rejects medical PDFs exceeding 200 pages, or fails to parse complex anatomical tables during the beta launch / **Then:** The extraction pipeline halts, the app generates no structured text, and the Diagnostic Loop completely breaks.
**Leading to:** 2 months of runway lost to failed onboarding and forcing a frantic pivot to a custom Marker fallback.
**Likelihood (1-5):** 5 / **Impact (1-5):** 4 (2 months)
**Mitigation:**
1. **Self-Host Fallback:** Deploy Marker (open-source) on Railway as a Python microservice for text-heavy PDFs ($20–30/month).
2. **UI Chunking:** Force users to select a 100-page range in the UI before hitting the API.
3. **Cache Bypass:** Heavily pre-warm the Global Knowledge Cache with top university files so early users bypass the MinerU step entirely.

### RISK 2: The "Blind Spot" Fallacy 
**Type:** Customer-facing
**If:** The "Riskiest Assumption" is wrong and medical students actually know exactly what they are weak at / **Then:** They will actively bypass the Diagnostic Quiz + Priority Map, demanding just a fast Anki card generator.
**Leading to:** 4 months of runway lost building a complex Compound AI workflow that the market doesn't want.
**Likelihood (1-5):** 3 / **Impact (1-5):** 5 (>6 months)
**Mitigation:**
1. **Halt F2 Code:** Write zero code for the Diagnostic Quiz until 5 customer discovery interviews are complete.
2. **The Kill Question:** Ask: *"How did you decide what to study first for your last exam?"*. 
3. **Funnel Tracking:** Instrument exact drop-off tracking between the Quiz Complete and Priority Map steps.

### RISK 3: Clinical Liability & Malpractice Precedent
**Type:** Regulatory
**If:** The sandboxed AI hallucinates a drug dosage or contraindication and a student applies it to a real hospital patient / **Then:** Synapsy is sued under the Air Canada precedent, where AI outputs are treated as binding company statements.
**Leading to:** 12 months of runway instantly wiped out (Fatal) due to legal action and university blacklisting.
**Likelihood (1-5):** 2 / **Impact (1-5):** 5 (Fatal)
**Mitigation:**
1. **Refusal Engineering:** Hardlock the system prompt: "Chỉ dùng nội dung file đã upload.".
2. **Source Locking:** Force every output to display the source reference (page/line) of the original PDF.
3. **Clickwrap Shield:** Add a mandatory checkbox on signup: "I am studying, not treating. AI outputs are not clinical advice.".

### RISK 4: API Rate Limit Bottleneck (Error 429)
**Type:** Vendor
**If:** All 50 beta users upload PDFs and trigger parallel GPT-4o-mini generation simultaneously on the night before an exam / **Then:** OpenAI throws a 429 Error, freezing card generation during the critical 48-hour PMF validation window.
**Leading to:** 1 month of runway lost to a burned early-adopter cohort and failed launch.
**Likelihood (1-5):** 4 / **Impact (1-5):** 3 (1 month)
**Mitigation:**
1. **Request Queues:** Implement an exponential backoff queue for the Stage 2 card generation.
2. **Dual-Vendor Fallback:** Wire Claude Haiku as an automatic fallback (it is 50% cheaper and has distinct rate limits).
3. **Progressive UI:** Render cards to the UI one by one as they generate, rather than waiting for `Promise.all`.

### RISK 5: Solo Founder Single Point of Failure
**Type:** Founder-bandwidth
**If:** The solo founder burns out, gets sick, or has an emergency for 3+ days during the launch sprint / **Then:** Critical bugs go unfixed, API limits aren't monitored, and investor updates are missed.
**Leading to:** 1.5 months of runway lost from missed PMF windows and delayed fundraising momentum.
**Likelihood (1-5):** 4 / **Impact (1-5):** 3 (1.5 months)
**Mitigation:**
1. **Main Branch Discipline:** Ensure `main` is always shippable; keep all WIP on branches.
2. **Daily Worklog:** Update `WORKLOG.md` at the end of every sprint with current state and next 3 actions.
3. **Meeting Buffers:** Mandate a 2-day code freeze before any investor meetings.

---

## ⚠️ THE WATCH ZONE (Secondary Risks)

### RISK 6: The "Zero-Tolerance" Hallucination Boycott
**Type:** Reputational
**If:** Traceability fails and a flashcard shows fabricated data / **Then:** A student tweets the error, triggering a viral boycott.
**Leading to:** 3 months of runway lost doing damage control.
**Mitigation:** Execute Incident Playbook (DM within 15 mins, founder apology tweet).

### RISK 7: Firebase Free Tier Blackout
**Type:** Vendor
**If:** 50 users upload 20MB PDFs simultaneously / **Then:** Firebase Spark Plan storage limits max out, freezing the onboarding loop.
**Leading to:** 0.5 months of runway burned fighting infrastructure fires.
**Mitigation:** Swap storage to Cloudflare R2 ($0.15/mo) and auto-delete PDFs after MinerU parses them.

### RISK 8: IP & PII Data Leak via Git/APIs
**Type:** Regulatory
**If:** API keys are committed to public Git, or PDFs with school PII are fed to non-enterprise LLMs / **Then:** Severe privacy violations occur, and API access is permanently revoked.
**Leading to:** 6 months of runway lost to institutional blacklisting.
**Mitigation:** Install `git-secrets` ($0) and integrate Helicone to audit logs.

### RISK 9: Cache Collision Burnout (AI Augmented)
**Type:** Customer-facing
**If:** The SHA-256 hash fails to distinguish between "Cardio_2025" and "Cardio_2026" PDFs / **Then:** Students are served the wrong Diagnostic Quiz from the Global Cache, failing their test prep.
**Leading to:** 2 months of runway lost from mass churn and manual database cleanup.
**Mitigation:** Hash bytecode + internal metadata (not just filename); add a manual "Re-process with AI" UI button.

### RISK 10: Premature Optimization Trap (AI Augmented)
**Type:** Founder-bandwidth
**If:** The founder spends 2 weeks over-engineering the Helicone logging and Cloudflare R2 pipeline before getting 10 real users / **Then:** The 13-day critical path stretches to a full month.
**Leading to:** 1 month of runway lost delaying the 50/50/48h PMF validation window.
**Mitigation:** Stick strictly to the 13-day critical path; defer R2 migration until Firebase hits 80% capacity.