# Vocal Biomarkers for Heart Failure — The Science & How to Build It

> Deep dive on the science, the evidence quality, and a concrete build plan. Companion to
> [IDEA-DEEPDIVE.md](./IDEA-DEEPDIVE.md). Short version: **yes, this is backed by good research —
> real clinical trials with published sensitivity numbers, plus a decade of adjacent validation
> in Parkinson's and depression. It is not speculative.**

---

## 1. The core idea in one sentence

Heart failure causes fluid to build up in the body (including the lungs and vocal tract) *days to
weeks* before a patient feels it or the scale moves — and that fluid subtly changes how the voice
sounds. AI can hear that change before the patient can.

---

## 2. The physiological mechanism (why the voice actually changes)

This isn't hand-waving — the AHF-Voice study lays out four concrete physiological pathways by
which pulmonary/systemic congestion alters phonation: [Frontiers / PMC12082658](https://pmc.ncbi.nlm.nih.gov/articles/PMC12082658/)

1. **Vocal fold edema** — volume overload swells the vocal folds, changing how they vibrate.
2. **Vocal tract swelling** — general edema shifts the resonance chambers, altering *formants*
   (the resonant frequency bands that shape vowel sound).
3. **Impaired lung function** — congestion reduces the air pressure driving the voice, lowering and
   destabilizing loudness/intensity and shortening how long someone can sustain a sound.
4. **Recurrent laryngeal nerve compression (Ortner's syndrome)** — an enlarged heart/vessels can
   press the nerve controlling the voice box, causing hoarseness.

**Net acoustic effect:** a hoarser voice, a narrower vocal range, a **higher and more irregular
fundamental frequency (raised jitter)**, shifted formants, and reduced/less stable intensity. These
are measurable, not subjective.

---

## 3. What exactly gets measured (the acoustic features)

The clinical-standard feature set is **eGeMAPS** (extended Geneva Minimalistic Acoustic Parameter
Set) — 28 low-level descriptors purpose-built for medical voice research. [Feature-tool review, arXiv 2025](https://arxiv.org/html/2506.01129v1) The ones that matter here:

| Feature | What it captures | Why HF changes it |
|---|---|---|
| **Jitter** | cycle-to-cycle *pitch* instability | vocal fold edema → irregular vibration |
| **Shimmer** | cycle-to-cycle *loudness* instability | unstable phonation from congestion |
| **HNR** (harmonics-to-noise ratio) | voice "clarity" vs breathy noise | swelling adds turbulence/noise |
| **F0 (fundamental frequency)** | pitch and its variability | raised & more irregular when congested |
| **Formants (F1–F3)** | resonance / vowel shaping | vocal-tract swelling shifts them |
| **Intensity / loudness** | air-pressure-driven volume | impaired lung function reduces it |
| **Phonation duration** | how long a sound is sustained | reduced when breathless |
| **MFCCs** | overall spectral "fingerprint" | general timbre change |

**Key conceptual point:** it works as *personal anomaly detection*, not population classification.
Each patient is their own control — you learn *their* healthy "dry" baseline, then watch for drift
toward "wet." That's why it's robust despite everyone's voice being different.

---

## 4. Is it backed by good research? (Yes — the evidence)

### The flagship: Cordio HearO™ studies
- **HearO Community Study:** **545 chronic-HF outpatients**, ~**660,000 voice recordings** (daily
  self-recording at home). [Healio, AHA 2023](https://www.healio.com/news/cardiology/20231113/smartphonebased-speech-analysis-app-may-predict-worsening-heart-failure) · [Medscape](https://www.medscape.com/viewarticle/998730)
- **Sensitivity 76–81%** for detecting heart-failure events, with detection on average **~18–24
  days before** the event. ~**70% sensitivity ~3 weeks ahead** in earlier analysis. [Medscape](https://www.medscape.com/viewarticle/998730)
- **The comparison that sells it:** the current home standard — **daily weight — has only ~20%
  sensitivity** and typically moves too late. Voice roughly **quadruples** the early-warning
  signal. [HF RPM meta-analysis](https://pmc.ncbi.nlm.nih.gov/articles/PMC12502459/)
- It's advancing to large prospective validation (e.g. a UCSF international blinded study of the
  HearO system). [ClinicalTrials/UCSF](https://clinicaltrials.ucsf.edu/trial/NCT06378632)

### Independent replication: AHF-Voice
- **131 acute-HF inpatients**, mean age 75, 86% NYHA III/IV, **3,072 recordings**, comparing
  "decompensated vs recompensated" voice — an independent academic group building the same
  wet-vs-dry model. [AHF-Voice, Frontiers 2025](https://pmc.ncbi.nlm.nih.gov/articles/PMC12082658/) Also the VAMP-HF cohort (European Heart Journal 2025).

### The deep foundation: voice-as-biomarker is a mature field
Voice biomarkers aren't new to HF — they're a decade-plus validated field, which is *why* this is
credible:
- **Parkinson's disease:** jitter, shimmer, HNR, and MFCCs distinguish patients from controls with
  **often >90% accuracy** in systematic reviews — the same features, the same pipeline. [Parkinson's ML review, PMC](https://pmc.ncbi.nlm.nih.gov/articles/PMC12649940/) · [Vocal features review, PMC](https://pmc.ncbi.nlm.nih.gov/articles/PMC11939921/)
- **Depression / mental health:** prosody, tone, and rhythm are validated markers of emotional
  state. [Voice & depression, arXiv 2024](https://arxiv.org/pdf/2411.11541)
- **Respiratory disease (COVID, COPD):** cough/voice classification has large public datasets.

**Verdict on evidence quality:** Strong and improving. There are real trials with hundreds of
patients and published sensitivity figures (76–81%), a clear physiological mechanism, independent
replication, and a mature adjacent literature. The honest caveats: most HF numbers come from one
company's studies (Cordio), large independent prospective validation is still in progress, and it's
not yet a cleared standalone diagnostic. That's a *startup opportunity*, not a red flag.

---

## 5. How to build it — the real pipeline

The production pipeline (what a real company builds):

```
Daily fixed-script audio  →  feature extraction (eGeMAPS)  →  personal baseline model
   (patient reads          (jitter, shimmer, HNR,           (anomaly / drift detection
    5 set sentences)         F0, formants, intensity)         vs the patient's own "dry" state)
                                                                      │
                                                             drift crosses threshold
                                                                      │
                                                       alert care team + trend explanation
```

- **Speech protocol:** the patient reads a **fixed set of sentences** daily (fixed text controls
  for content so only *voice quality* varies — this is how HearO does it). ~30 seconds.
- **Feature extraction (all open-source, runs locally):**
  - **openSMILE** → the eGeMAPS feature set (the clinical standard).
  - **Praat via `parselmouth`** (Python) → validated jitter, shimmer, HNR, pitch, formants.
  - **`librosa` / `torchaudio`** → MFCCs, spectral features.
- **Model:** because it's personal drift detection, you don't need a giant neural net — a per-patient
  baseline + anomaly score (z-score / isolation forest / simple classifier on the feature vector)
  is the honest core. The *data*, not the model, is the moat.

---

## 6. How to build it for the HACKATHON (feasible in ~20h)

You **cannot** train a real clinically-valid HF model in a weekend — there's no labeled patient
voice dataset lying around. So build the **real pipeline on illustrative audio**, and be honest
about it. This is legitimate and still impressive because the *feature extraction is real*.

**What's real vs. simulated:**
- ✅ **Real:** live audio capture, real openSMILE/parselmouth feature extraction, real jitter/
  shimmer/HNR/intensity computed on stage, real drift detection vs a baseline, real escalation.
- 🟡 **Simulated:** the "wet vs dry" labels — you seed a healthy baseline and a "congested" sample.

**Three ways to get a convincing wet/dry contrast for the demo:**
1. **Physiological proxy (best/honest):** record a normal baseline, then record after light
   exertion / breath-holding / speaking lying down — genuinely shifts intensity, phonation
   duration, and jitter. You show the features *actually move* live.
2. **Seeded samples:** pre-record a "week of baseline" + a "day 21 congested-sounding" clip; run
   real extraction live so the audience watches HNR drop and jitter rise past the threshold.
3. **Adjacent open data:** use a public pathological-voice or respiratory dataset to show the
   classifier separating healthy vs affected — proof the pipeline discriminates.

**The demo moment:** patient does the daily voice check-in → a live chart shows jitter/HNR drifting
over "3 weeks" → the line crosses the threshold → the app drafts a red-flag alert to the nurse.
Narrate: *"Daily weight would have caught this with 20% sensitivity, and later. Her voice caught it
three weeks early."*

**Framing that survives a clinician judge:** "This is an early-stage prototype of a
clinically-validated *direction* — Cordio's HearO showed 76–81% sensitivity and ~3 weeks lead time
in 545 patients; we've built the real feature-extraction and drift-detection pipeline and are
demonstrating the signal. The next step is a labeled clinical dataset." Honest, and it shows you
know the literature cold.

---

## 7. The elegant fusion with the voice companion

Here's why this pairs perfectly with Idea 1 (the GLP-1 / metabolic companion): **the companion is
already having a daily voice conversation with the patient.** That conversation *is* the audio
sample. So the biomarker analysis runs **passively, in the background, on speech the patient is
already producing** — no extra step, no fixed-script chore. "It doesn't just ask how you feel — it
hears how you feel." That's the wow moment, and it's a genuinely novel combination: a
conversational adherence agent that doubles as a passive physiological sensor.

---

## 8. Honest strengths & weaknesses recap

**Strengths:** real mechanism, real trials (76–81% sensitivity, ~3-week lead), frictionless, the
data becomes a defensible moat, and it beats the current standard (weight, ~20%) by a wide margin.
Highest novelty of anything on the table.

**Weaknesses:** most HF evidence is from one company so far; large independent prospective
validation is ongoing; it's a regulated medical-device path long-term; and in a hackathon the
"working model" is necessarily simulated on illustrative audio. Manage this with honest framing.

---

## Primary sources
- Mechanism & independent study: [AHF-Voice, Frontiers/PMC 2025](https://pmc.ncbi.nlm.nih.gov/articles/PMC12082658/)
- HearO clinical results: [Healio AHA 2023](https://www.healio.com/news/cardiology/20231113/smartphonebased-speech-analysis-app-may-predict-worsening-heart-failure) · [Medscape](https://www.medscape.com/viewarticle/998730) · [UCSF validation trial](https://clinicaltrials.ucsf.edu/trial/NCT06378632)
- Weight-monitoring sensitivity baseline: [HF RPM meta-analysis, PMC](https://pmc.ncbi.nlm.nih.gov/articles/PMC12502459/)
- Voice-biomarker foundation (Parkinson's): [ML review, PMC](https://pmc.ncbi.nlm.nih.gov/articles/PMC12649940/) · [Vocal features review, PMC](https://pmc.ncbi.nlm.nih.gov/articles/PMC11939921/)
- Depression/voice: [arXiv 2024](https://arxiv.org/pdf/2411.11541)
- Feature-extraction toolchain: [Comparative eval of tools, arXiv 2025](https://arxiv.org/html/2506.01129v1)
