# Undertone - the listening layer

> Voice as a continuous vital sign for chronic metabolic disease. A daily 60-second voice
> check-in doubles as a passive acoustic sensor: it extracts biomarkers of muscle loss (and, on
> the roadmap, cardiac / glycemic / renal decline), detects drift over time, and escalates an
> explainable, cited summary to a clinician — human-in-the-loop, never an autonomous diagnosis.
>
> Built for the ReimagineHealth hackathon (eMed × OpenAI). *Working name — see [NAMES.md](./NAMES.md).*

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
then switch to the **Clinician** tab to see the explainable escalation.

## Repo contents
- `demo/` — the working prototype (patient dashboard + clinician escalation view)
- `MARKET-RESEARCH.md`, `VOCAL-BIOMARKER-SCIENCE.md`, `DISEASE-LANDSCAPE.md` — the cited evidence base
- `DIFFERENTIATION.md`, `STARTUP-LANDSCAPE-AND-FRICTION.md`, `CRITICAL-APPRAISAL-KIDNEY.md` — competitive & defensibility analysis
- `VISION-PIVOT.md`, `ARCHITECTURE-DECISIONS.md` — the concept and technical decisions
- `CUE-CARDS.md` — 3-minute pitch + demo narration
- `GAMMA-DECK-PROMPT.md` — prompt to generate the pitch deck
- `NAMES.md` — naming options

## Status
Prototype. Muscle-integrity voice signal is demonstrated live; cardiac / glycemic / renal are
roadmap. Acoustic feature extraction is real; the wet/dry training labels are simulated for the
demo (see [VOCAL-BIOMARKER-SCIENCE.md](./VOCAL-BIOMARKER-SCIENCE.md) for the honest framing).
