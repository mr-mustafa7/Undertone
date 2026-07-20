# Voice Biomarkers — Who's Already Building This, and Why It's Not Mainstream Yet

> Companion to [VOCAL-BIOMARKER-SCIENCE.md](./VOCAL-BIOMARKER-SCIENCE.md). Short answer: **yes,
> real startups exist, one is mid-way through FDA breakthrough-device review, and the friction is
> not the science — it's regulatory cost, clinician trust, and reimbursement. That gap is exactly
> the hackathon's opening, not a reason to avoid the idea.**

---

## 1. Who's already building it

### Cordio Medical (HearO™) — the direct HF competitor
- **$20M raised** (Ceros Capital, Peregrine Ventures). [PRNewswire](https://www.prnewswire.com/il/news-releases/cordio-medical-raises-18-million-in-funding-from-ceros-and-peregrine-ventures-to-accelerate-growth-301576045.html)
- **FDA Breakthrough Device designation** granted — this is a fast-track status, not clearance
  itself; it means the FDA agrees the unmet need is real and will prioritize review, but the
  company is still mid-pipeline, not commercially cleared in the US yet. [Medical Device Network](https://www.medicaldevice-network.com/news/coredio-ai-heart-failure-software-secures-breakthrough-designation/)
- Partnered with **AstraZeneca** for a European pilot (General Hospital of Valencia, Valencian
  Society of Cardiology). [Times of Israel](https://www.timesofisrael.com/spotlight/speak-and-be-saved-israeli-app-detects-heart-failure-weeks-before-attack/)
- **Status: still pre-mainstream** — years into clinical validation, not yet a standard-of-care
  tool a GP or cardiologist reaches for.

### The adjacent voice-biomarker field (proves the category, not HF-specific)
- **Sonde Health** — vocal biomarkers for respiratory/mental/cognitive health from everyday speech.
- **Canary Speech** — voice analysis for neurological/psychiatric/cognitive disorders.
- **Kintsugi** — raised **$20M** for voice-biomarker mental-health screening, but is **shutting
  down commercial operations as of February 2026**, open-sourcing its research instead. [Fierce Healthcare](https://www.fiercehealthcare.com/health-tech/kintsugi-banks-20m-scale-voice-biomarker-based-mental-health-tech) [GlobeNewswire](https://www.globenewswire.com/news-release/2025/09/19/3153317/28124/en/Voice-Vocal-Biomarkers-Market-to-Quadruple-in-Size-by-2035-Analysis-of-Key-Industry-Trends-Opportunities-and-Challenges-with-Competition-Profiling.html)
- **Market size:** ~$1.08B (2024) → projected **$5.4B by 2035** — real but still small and early.
  [GlobeNewswire market report](https://www.globenewswire.com/news-release/2025/09/19/3153317/28124/en/Voice-Vocal-Biomarkers-Market-to-Quadruple-in-Size-by-2035-Analysis-of-Key-Industry-Trends-Opportunities-and-Challenges-with-Competition-Profiling.html)

**Reading the tea leaves:** funding exists, the science is real, one company is furthest along in
exactly your target condition (HF) — and even the best-funded, best-positioned player (Cordio) is
still not a mainstream clinical tool years after founding. **Kintsugi shutting down is the
important signal**: strong tech + funding still isn't enough without a reimbursement/adoption path.
This tells you *why* it isn't mainstream, not that it's a dead end.

---

## 2. So why isn't this mainstream, if the science works?

Four real, well-documented barriers — none of them "the AI doesn't work."

### a) Regulatory cost and time, not science
- FDA now treats any software that drives a clinical decision a user "can't reasonably override"
  as a **medical device** requiring clearance. [FDA 2024 CDS policy](https://intuitionlabs.ai/articles/fda-digital-health-technology-guidance-requirements)
- Realistic cost: **$250K (simple) to $2M+** for a Class C device with clinical validation, and
  crucially **the cost is dominated by evidence generation (running trials), not the FDA review
  fee itself.** [Innolitics](https://innolitics.com/articles/fda-submission-market-dynamics/)
- Review itself is comparatively fast (median 162 days for ML-enabled devices) — **the bottleneck
  is the years of clinical trials needed *before* you can even submit**, not the paperwork.
  [PMC 2024 ML devices](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC12730494/)

### b) Clinician trust
- A 2025 review states plainly: **"the primary barrier to integrating voice biomarkers into
  clinical settings is the lack of trust among clinicians."** [Frontiers 2025](https://pmc.ncbi.nlm.nih.gov/articles/PMC12267164/)
- Compounding this: symptom heterogeneity. One study of 3,703 depression patients found **1,030
  distinct symptom profiles** — a reminder that a single acoustic signature doesn't generalize
  cleanly across every patient, which makes clinicians (rightly) cautious about a black-box alert.

### c) Reimbursement — nobody pays for it yet
- Even a clinically excellent tool stalls without a CPT billing code or payer contract — this is
  the generic medtech-failure pattern from [IDEAS.md](./IDEAS.md), and voice biomarkers are
  squarely in it. [NEJM AI, payment systems](https://ai.nejm.org/doi/full/10.1056/AIpc2500871)
- Without reimbursement, a hospital can *want* the tool and still not buy it — there's no budget
  line for it.

### d) Data scarcity and generalizability
- Every serious review flags **data scarcity and model generalizability** as ongoing technical
  barriers — voices vary by language, accent, recording device, and background noise, and most
  labeled clinical datasets are still small (hundreds, not tens of thousands, of patients).
  [Frontiers 2025](https://pmc.ncbi.nlm.nih.gov/articles/PMC12267164/)

---

## 3. What this means for the hackathon pitch (turn the friction into your differentiator)

This is good news for you, not bad news — **the friction is exactly what the judges (investors +
eMed) will want to see you understand**, and it's exactly where a fresh approach can look smarter
than the incumbent:

1. **Don't chase FDA clearance as your MVP.** Position the voice signal as a **screening /
   triage nudge that a human clinician reviews**, not an autonomous diagnosis — this sidesteps
   the "software as medical device" trap for a v1 and gets you deployable *now*, not in 3 years.
   Say this explicitly in the pitch — it shows regulatory literacy.
2. **Build trust in, don't bolt it on.** Cordio's own barrier is clinician trust — so make your
   escalation **explainable** ("flagged because jitter rose 18% and HNR dropped over 5 days," not
   a black-box score) and always **human-in-the-loop**. That's a concrete, buildable differentiator.
3. **The reimbursement story is your adherence data, not the biomarker alone.** Voice-biomarker
   companies stall because insurers don't have a line item for "vocal analysis." But payers
   *already* pay to prevent GLP-1 discontinuation and HF readmission — so pitch the **adherence
   companion** as the sellable product, with the biomarker as an embedded feature inside a
   fundable product, not a standalone device seeking its own reimbursement code. This is *exactly*
   why the fusion concept (companion + passive biomarker) is smarter than a standalone voice
   biomarker startup.
4. **Kintsugi's lesson:** a great biomarker without a distribution channel dies. Your companion
   *is* the distribution channel — it's already in a daily relationship with the patient, so the
   biomarker rides along for free instead of needing its own adoption motion.

**One line for the pitch:** *"Voice biomarkers work — Cordio's already shown 80% sensitivity three
weeks early. What's stalled is standalone regulatory and reimbursement paths. We're not launching
a medical device — we're embedding an explainable, human-reviewed signal inside a product patients
and payers already need for adherence. The science is proven; we fixed the distribution."*

---

## Source index
- Cordio: [funding](https://www.prnewswire.com/il/news-releases/cordio-medical-raises-18-million-in-funding-from-ceros-and-peregrine-ventures-to-accelerate-growth-301576045.html) · [FDA breakthrough status](https://www.medicaldevice-network.com/news/coredio-ai-heart-failure-software-secures-breakthrough-designation/) · [AstraZeneca pilot](https://www.timesofisrael.com/spotlight/speak-and-be-saved-israeli-app-detects-heart-failure-weeks-before-attack/)
- Field/competitors: [Sonde/Canary/Kintsugi market overview](https://www.fiercehealthcare.com/health-tech/kintsugi-banks-20m-scale-voice-biomarker-based-mental-health-tech) · [market size report](https://www.globenewswire.com/news-release/2025/09/19/3153317/28124/en/Voice-Vocal-Biomarkers-Market-to-Quadruple-in-Size-by-2035-Analysis-of-Key-Industry-Trends-Opportunities-and-Challenges-with-Competition-Profiling.html)
- Barriers: [clinician trust / validation review, Frontiers 2025](https://pmc.ncbi.nlm.nih.gov/articles/PMC12267164/) · [FDA cost/timeline, Innolitics](https://innolitics.com/articles/fda-submission-market-dynamics/) · [ML device stats 2024, PMC](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC12730494/) · [reimbursement, NEJM AI](https://ai.nejm.org/doi/full/10.1056/AIpc2500871)
