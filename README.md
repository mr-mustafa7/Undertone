# Undertone — the listening layer

> Voice as a continuous vital sign for chronic metabolic disease. A daily 60-second voice
> check-in doubles as a passive acoustic sensor: it extracts biomarkers of muscle loss (and, on
> the roadmap, cardiac / glycemic / renal decline), detects drift over time, and escalates an
> explainable, cited summary to a clinician — human-in-the-loop, never an autonomous diagnosis.
>
> Built for the ReimagineHealth hackathon (eMed × OpenAI). *Working name — see [pitch/NAMES.md](./pitch/NAMES.md).*

**Demo video:**
https://github.com/user-attachments/assets/7afe29bd-d086-4188-9018-7f0ce31bc703

**Using Prometheux as the reasoning backend** — here is the generated ontology and data flow
(full Vadalog program in [pitch/PROMETHEUX-ONTOLOGY-PROMPT.md](./pitch/PROMETHEUX-ONTOLOGY-PROMPT.md)):

<img width="714" height="582" alt="Undertone ontology graph" src="https://github.com/user-attachments/assets/1b9b8ca7-c6fd-4bd4-86e1-bdbf06b928ca" />
<img width="385" height="610" alt="Undertone data flow" src="https://github.com/user-attachments/assets/658d9369-a4e7-4816-98e6-cf113b984e9e" />
<img width="1230" height="673" alt="Undertone escalation derivation trace" src="https://github.com/user-attachments/assets/e1ea2e55-0c01-41e5-aeff-3a4aafe1e5a6" />

## The one-line thesis
50% of metabolic-programme patients quit within a year; the proven fix is frequent contact (49×
persistence with 5+ visits) but the system can't scale it. Voice — proven to detect deterioration
weeks early (Cordio HearO: 76–81% sensitivity vs. ~20% for daily weight) — turns a conversation
patients already have into the monitoring the system can't otherwise deliver.

## Run the demo
```bash
cd demo
npx serve . -l 4599
# open http://localhost:4599
```
The demo is a single self-contained `demo/index.html` — no build step. It uses the real Web Audio
API for live acoustic feature extraction (energy / zero-crossing / pitch), with a graceful
sample-audio fallback if no microphone is available. Demo flow: **Talk → Snap → Hear + Escalate**,
then switch to the **Clinician** view to see the explainable escalation, patient panel, and audit
log (role-scoped — no cross-patient search from the patient view).

## Repo contents
- `demo/` — the working prototype (patient dashboard + clinician escalation view, role-based access)
- `research/` — the cited evidence base, competitive analysis, and concept development:
  `MARKET-RESEARCH.md`, `VOCAL-BIOMARKER-SCIENCE.md`, `DISEASE-LANDSCAPE.md`,
  `DIFFERENTIATION.md`, `STARTUP-LANDSCAPE-AND-FRICTION.md`, `CRITICAL-APPRAISAL-KIDNEY.md`,
  `VISION-PIVOT.md`, `ARCHITECTURE-DECISIONS.md`, and more
- `pitch/` — presentation materials: `CUE-CARDS.md` (3-minute pitch + demo narration),
  `GAMMA-DECK-PROMPT.md` (deck generation prompt), `PROMETHEUX-ONTOLOGY-PROMPT.md`
  (the ontology prompt **and the real generated Vadalog output**, above), `NAMES.md` (naming options)

## Status
Prototype. Muscle-integrity voice signal is demonstrated live, backed by a real Prometheux/Vadalog
reasoning engine (see the ontology above); cardiac / glycemic / renal signals are modeled as
roadmap stubs in the ontology, currently unpopulated. Acoustic feature extraction is real; the
wet/dry training labels are simulated for the demo (see
[research/VOCAL-BIOMARKER-SCIENCE.md](./research/VOCAL-BIOMARKER-SCIENCE.md) for the honest framing).
