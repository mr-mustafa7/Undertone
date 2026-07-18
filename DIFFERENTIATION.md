# Why Ours Is Different From HearO (and Other Voice-Biomarker Startups)

> Direct answer to: "others will have this idea, it's already a thing." Companion to
> [STARTUP-LANDSCAPE-AND-FRICTION.md](./STARTUP-LANDSCAPE-AND-FRICTION.md). Honest framing: the
> *acoustic science* (jitter/shimmer/HNR detecting congestion) is not ours to claim as novel —
> Cordio proved it. What's genuinely different is **who you're screening, how you capture the
> voice, and what you do with it alongside the words being spoken.** Three real, evidence-backed
> differences, not marketing spin.

---

## Difference 1 — A different population: upstream screening vs. downstream monitoring

**HearO's cohort:** already-diagnosed chronic heart-failure outpatients. The AHF-Voice study: mean
age 75, **86% in NYHA class III/IV** (advanced, symptomatic disease). [AHF-Voice, PMC](https://pmc.ncbi.nlm.nih.gov/articles/PMC12082658/)
This is a narrow population that is *already captured* by cardiology — they know they're sick, they
have a cardiologist, and they've opted into a monitoring program.

**Our target population:** patients on GLP-1 / obesity / T2D programmes — a vastly larger group,
most of whom **do not have a diagnosed cardiac condition** but are at meaningfully elevated risk of
one specifically relevant to voice biomarkers.

- Obesity is a **major, well-established risk factor for HFpEF** (heart failure with preserved
  ejection fraction) via inflammation, neurohormonal activation, and mechanical strain. [PMC 2025](https://pmc.ncbi.nlm.nih.gov/articles/PMC12081111/)
- GLP-1 therapy in patients with obesity + HFpEF cuts **combined HF hospitalization/mortality by
  40%** in a 22,000-patient evidence base — meaning this population's cardiac risk is real and
  already clinically recognized, but **HFpEF is notoriously underdiagnosed** because it's silent
  until it isn't. [AJMC](https://www.ajmc.com/view/glp-1s-reduced-adverse-heart-failure-events-by-40-in-patients-with-hfpef) · [systematic review, 2024–25](https://pubmed.ncbi.nlm.nih.gov/40937512/)

**The wedge:** HearO monitors people who are *already* diagnosed and being watched. We'd be
screening a much larger population who are at real, evidenced cardiac risk **but currently get zero
cardiac monitoring at all**, because nobody is looking — they're in an obesity/diabetes pathway, not
a cardiology one. That's not competing with HearO; it's addressing the population *before* HearO's
population exists. Complementary, not copycat.

---

## Difference 2 — Passive, incidental capture vs. a dedicated daily task

HearO requires patients to open the app and **read five prespecified sentences every morning** as a
standalone task. [Healio](https://www.healio.com/news/cardiology/20231113/smartphonebased-speech-analysis-app-may-predict-worsening-heart-failure) Notably, this actually works well for *their* cohort — **83.5% adherence**, because
NYHA III/IV heart-failure patients are highly motivated; they feel sick and know a missed early
warning means a hospital stay. [tctmd / COMMUNITY-HF](https://www.tctmd.com/news/voice-analysis-picks-patients-risk-hf-hospitalization-community-hf)

**That motivation doesn't exist in our population.** GLP-1/metabolic patients are largely
asymptomatic day-to-day, and this exact population is the one where digital-health engagement
collapses — **>50% quit the drug within a year**, and real-world app cohorts see as low as **2%
sustained continuous use**. [MARKET-RESEARCH.md](./MARKET-RESEARCH.md) A second, separate "please
read these sentences daily" app would very likely fail in this population even if the acoustic
science is identical.

**Our approach:** the voice sample isn't a separate task — it's a byproduct of a conversation the
patient is *already* having for a different, motivating reason (their adherence coach checking in
about nausea, meals, meds). The biomarker analysis rides on infrastructure that has to exist anyway.
This directly solves the exact engagement problem that kills point-solution monitoring apps, without
asking the patient for anything extra.

---

## Difference 3 — Fusing *what* they say with *how* they say it

From the published methodology, HearO's model is **purely acoustic** — it analyzes fixed-script
recordings for signal, with no use of conversational content (there isn't any; the sentences are
scripted, not a real conversation).

**Our companion has both channels simultaneously:** the semantic content of a real conversation
("I feel fine today, no issues") *and* the acoustic drift underneath it in the same moment. This
enables **contradiction detection** — if the patient says they're fine but jitter/HNR show
congestion-pattern drift, that mismatch is a stronger, more specific signal than either channel
alone. This is an active, real 2025 research direction:

- Acoustic-textual mismatch is being studied directly for mood disorders — capturing the gap
  between what someone says and how they sound as a distinct diagnostic signal. [arXiv 2412.18614](https://arxiv.org/pdf/2412.18614)
- Multimodal fusion of speech content + acoustic features is an active benchmark area across
  depression, cognitive impairment, and psychiatric severity estimation in 2025 literature.
  [SpeechDx benchmark](https://arxiv.org/pdf/2606.17339) · [Feature fusion, schizophrenia severity](https://arxiv.org/pdf/2411.06033)

A scripted-sentence app **structurally cannot do this** — there's no free conversational content to
check the acoustic signal against. A conversational companion can, by design.

*(Honest scope note: the HF congestion mechanism and the mood/depression contradiction research are
each independently well-evidenced; applying contradiction-detection specifically to cardiac
congestion is a reasonable, novel extension, not itself a published clinical finding. Frame it in
the pitch as the innovative combination, not as an already-proven result.)*

---

## Bringing it together — the one-paragraph answer to "won't others have this idea"

*"The acoustic science isn't new — Cordio's HearO proved voice can catch heart failure ~3 weeks
early. What's new is where we point it: not at patients who are already diagnosed and already
motivated to do a daily monitoring chore, but at the much larger population of obesity and diabetes
patients who are at real, evidenced cardiac risk and get zero cardiac monitoring today. We don't ask
them for a separate task — we listen inside a conversation that already has to happen for their
primary condition, which solves the engagement collapse that kills point-solution monitoring apps.
And because it's a real conversation, not a script, we can check what they say against how they
sound — a fusion signal HearO's fixed-sentence design can't produce. Same underlying science,
completely different population, delivery model, and signal — and it rides inside a product that's
already fundable for adherence, which sidesteps the reimbursement wall that's stalled every
standalone voice-biomarker company so far."*

---

## What this means for the build/demo

- Persona: **not** an elderly, already-diagnosed HF patient. Use an obesity/T2D patient on a GLP-1
  programme — the eMed-relevant persona — with an *undiagnosed*, elevated cardiac risk (mention the
  HFpEF link explicitly; it's your clinical justification for even listening for this signal).
- Demo the **contradiction moment** specifically: patient says "I'm okay" during the check-in →
  screen shows the acoustic trend disagreeing → escalation triggered *because of the mismatch*, not
  just a threshold crossing. That's the single most differentiating beat you can show live.
- One slide should name HearO directly and make the population/delivery/fusion distinction — judges
  who know the space (and eMed likely does) will respect that you know your competition and didn't
  stumble into the same idea by accident.
