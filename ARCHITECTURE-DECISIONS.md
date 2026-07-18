# Architecture Decisions — Hardware, Explainability, Reasoning Backend, GLP-1 Voice Signals

> Answers four build-critical questions. Companion to
> [DIFFERENTIATION.md](./DIFFERENTIATION.md) and [VOCAL-BIOMARKER-SCIENCE.md](./VOCAL-BIOMARKER-SCIENCE.md).
> **Headline finding: voice biomarkers for GLP-1-specific issues (muscle loss, dehydration) are
> independently research-backed — this is not just "borrowing HearO's cardiac trick." That
> materially strengthens the pitch.**

---

## Q1 — Phone or a dedicated voice-analysis device?

**Answer: phone. A dedicated device would be a strategic mistake, and the science backs the phone.**

- Direct validation: smartphone recordings (even without extra hardware) are **highly correlated
  with clinical-grade/"gold standard" equipment** for exactly the metrics this idea needs —
  fundamental frequency, jitter, shimmer, noise-to-harmonic ratio, cepstral peak prominence.
  [PMC 2024 validity study](https://pmc.ncbi.nlm.nih.gov/articles/PMC12379677/) · [Smartphone vs gold standard, PMC](https://pmc.ncbi.nlm.nih.gov/articles/PMC10545813/)
- **HearO itself is smartphone-only** — "requires no sensors or hardware other than a
  smartphone" — and still hit 76–81% sensitivity across 545 patients / 660,000 recordings. That's
  a direct existence proof at scale for this exact use case.
- Nuance from the literature: *perturbation* measures (jitter/shimmer) show more device-to-device
  variation than spectral measures, so the best-practice refinement is a **cheap clip-on headset
  mic to fix mouth-to-mic distance**, or software-side calibration (regression normalization for
  the phone model) — not a bespoke device. [JSLHR 2024](https://pubs.asha.org/doi/10.1044/2024_JSLHR-23-00759)

**Why a dedicated device would actively hurt the pitch:**
1. It reintroduces the exact friction your differentiation argument eliminates — "another gadget
   to carry/charge" is precisely the adoption failure mode that kills point-solution monitoring
   apps (see [DIFFERENTIATION.md](./DIFFERENTIATION.md) — passive capture is your wedge).
2. Hardware pushes you toward Class II/III medical-device regulatory classification much faster
   and more expensively ($250K–$2M+, mostly evidence generation) — the exact trap that's stalled
   Cordio and killed Kintsugi (see [STARTUP-LANDSCAPE-AND-FRICTION.md](./STARTUP-LANDSCAPE-AND-FRICTION.md)).
3. It adds manufacturing/supply-chain complexity no hackathon team (or seed-stage startup) needs.

**Recommendation:** phone mic, captured inside the existing voice check-in. Add a lightweight
recording-quality nudge ("find a quiet spot") and, later, per-device calibration — never a device.

---

## Q2 — Should an agent show its reasoning for doctor approval?

**Answer: yes, and the literature says this is exactly the right instinct — with one important caveat.**

- A 2024–2025 evidence review found **8 themes drive clinician trust** in AI decision support:
  transparency, workflow integration, clinical reliability, validation, and — critically —
  **clinician control**, not just an explanation. [MDPI meta-analysis, 2025](https://www.mdpi.com/2227-9032/13/17/2154)
- Emerging best practice is exactly what you're describing: an **auditable, source-verified
  framework** where AI recommendations carry explicit **data provenance** so a clinician can
  verify, not just trust. [PMC 2025, auditable clinical AI](https://pmc.ncbi.nlm.nih.gov/articles/PMC12913532/)
- **The caveat, and it matters:** a systematic review found that explanations reliably **increase
  perceived trust** but **do not reliably improve decision quality** — and can sometimes increase
  harmful agreement with an AI that's wrong. [ISJ trend, human-centered eval](https://www.isjtrend.com/article_243223.html)
  → **Design implication:** the explanation must be a real, checkable derivation (which facts,
  which threshold, which rule), not a persuasive-sounding LLM paragraph that *feels* rigorous but
  is a post-hoc rationalization. This is the difference between genuine and cosmetic explainability
  — and it's exactly why the architecture in Q3 matters.

---

## Q3 — Could Prometheux/Vadalog be the reasoning backend?

**Answer: yes, and it's a strong fit — arguably better than an LLM alone for the "show your
reasoning" requirement in Q2.** *(Note: I tried to pull live docs from the Prometheux MCP tools to
confirm current specifics and the connection timed out twice — the description below is from
established knowledge of the Vadalog/Datalog+/- reasoning model and your own [[Mizan Prometheux
pipeline]] project experience; verify exact syntax against the docs when you actually build.)*

**Why it's the right kind of tool for this specific problem:**
- Vadalog implements a logic/rule-based reasoning engine (Warded Datalog+/-), not a neural
  black box. Every conclusion it reaches comes with a **derivation trace** — the exact chain of
  facts and rules that produced it. That is *structurally* different from asking an LLM to explain
  itself after the fact, and it directly answers the literature's core critique above: the
  explanation **is** the reasoning, not a rationalization of it.
- Clean division of labor for this project:
  - **LLM/voice pipeline** handles the messy, unstructured part — turning conversation + audio into
    structured facts (`jitter_delta = 18%`, `hnr_delta = -6%`, `self_report = "feeling fine"`,
    `days_observed = 5`).
  - **Vadalog ontology** encodes the clinical rules from the cited literature as explicit,
    auditable logic — e.g. *"IF jitter_delta > threshold AND hnr_delta < threshold OVER N days
    AND self_report contradicts acoustic_trend THEN escalate, citing [HearO congestion thresholds]."*
  - The **output shown to the doctor is the trace itself** — which facts, which rule, which
    citation-backed threshold — not a sentence the LLM generated.
- This is a legitimate, current architecture pattern (neuro-symbolic AI: LLM for perception,
  symbolic engine for auditable decision logic) — and it's a stronger technical answer than "we
  asked GPT to explain itself," which a clinician judge may reasonably distrust per Q2's caveat.

**Practical hackathon scope:** don't try to build a full ontology in 20h. Encode a **small, real
rule set (3–5 rules)** covering the demo's escalation moment, run it live end-to-end, and show the
derivation trace on the clinician screen. That's enough to prove the architecture and make the
explainability claim concrete rather than hand-waved.

---

## Q4 — Can voice detect GLP-1-specific things, or only cardiac (HearO's territory)?

**Answer: yes — and this is the most important finding of this research pass.** There are two
*independently published, GLP-1-relevant* voice mechanisms beyond the cardiac one, which means the
pitch is no longer "we borrowed HearO's trick" — it's "we built a voice panel tailored to what
actually happens to GLP-1 patients."

### a) Muscle loss / frailty — directly maps to the 40–60% lean-mass problem
- A 2024 JMIR cross-sectional study identified **4 frailty phenotypes from voice**, including a
  distinct **"sarcopenia-based frailty"** phenotype. Two acoustic parameters — zero-crossing rate
  (A1) and variation in peaks/valleys (A2) — were specifically identified as diagnosing "a frailty
  phenotype associated with **insufficient energy or reduced muscle function.**" [JMIR 2024](https://www.jmir.org/2024/1/e58466) · [PMC](https://pmc.ncbi.nlm.nih.gov/articles/PMC11584546/)
- A **January 2025** JMIR Medical Informatics paper built and validated an ML classifier
  predicting frailty status from voice in community-dwelling older adults. [JMIR Med Inform 2025](https://medinform.jmir.org/2025/1/e57298)
- **Why this matters for your pitch:** 40–60% of GLP-1 weight loss is lean muscle mass, and today
  the only way to detect that is a DEXA scan or bioimpedance test — nothing a patient does at
  home. A voice signal that tracks toward the sarcopenia-frailty phenotype during a GLP-1 program
  would be a **genuinely novel, non-invasive early-warning for a problem specific to this drug
  class** — not a borrowed cardiac signal at all.

### b) Hydration / GI-side-effect risk — directly maps to nausea/vomiting/diarrhea
- Vocal fold tissue mechanics (elasticity, viscosity) are **directly linked to hydration**, an
  effect confirmed via MRI. [PMC](https://pmc.ncbi.nlm.nih.gov/articles/PMC6283588/) · [PMC](https://pmc.ncbi.nlm.nih.gov/articles/PMC2925668/)
- A controlled study giving healthy adults a diuretic (reducing systemic hydration) found a
  **23% increase in phonation threshold pressure within 5–12 hours** — i.e., dehydration
  measurably changes vocal effort *fast*. [ASHA / hydration & voice acoustics](https://pubs.asha.org/doi/10.1044/cicsd_36_F_142)
- **Why this matters:** GI side effects (nausea 25–44%, vomiting 8–24%, diarrhea 19–30% — from
  [MARKET-RESEARCH.md](./MARKET-RESEARCH.md)) plus appetite suppression put GLP-1 patients at real
  dehydration risk. A hydration-sensitive voice signal, picked up in the same daily conversation,
  could flag a bad side-effect episode before it becomes an ER visit.

### c) Mood / motivational drop — an adherence-risk early warning
- Prosody and vocal "flatness" are well-established depression/low-affect markers (established
  earlier in [MARKET-RESEARCH.md](./MARKET-RESEARCH.md)'s AI-trends discussion). Since frustration
  and low motivation typically *precede* a patient's decision to quit their programme, a drop in
  vocal energy during check-ins is a plausible early adherence-risk signal, independent of any
  specific side effect.

### Honest scope note
No published study has tested vocal biomarkers **in a GLP-1 population specifically** — these are
two independently well-evidenced mechanisms (frailty/sarcopenia voice research; hydration/voice
research) applied by reasonable extension to a new, relevant context. That's legitimate scientific
reasoning (established mechanism + clearly relevant population = testable hypothesis), and it
should be pitched exactly that way: **"here is the evidence base, here is why we believe it
transfers, here is what we'd validate first"** — not as an already-proven result.

### What this changes about the pitch
You now have a **four-signal voice panel**, each tied to a distinct, cited mechanism, each mapped
to a real GLP-1 failure mode:

| Signal | Mechanism | GLP-1 failure mode it targets |
|---|---|---|
| Frailty markers (A1/A2, zero-crossing/peak variation) | Sarcopenia reduces muscle-driven vocal control | The 40–60% muscle-loss problem |
| Hydration markers (phonation effort, jitter) | Vocal fold tissue hydration | GI-side-effect dehydration risk |
| Prosody / affect | Established mood-voice link | Adherence-risk / motivation drop |
| Congestion markers (jitter, HNR, formants) | Fluid affects vocal tract/fold | Undiagnosed HFpEF risk (obesity link) |

This is a stronger differentiation than anything in [DIFFERENTIATION.md](./DIFFERENTIATION.md)
alone — it's not "HearO's mechanism, different population," it's "a voice panel purpose-built for
what actually goes wrong on a GLP-1 programme, with the cardiac signal as one component among four."
**For the hackathon, pick one signal to actually implement live (muscle/frailty is likely the most
on-brand and least crowded) and name the other three on the architecture slide as the roadmap.**
