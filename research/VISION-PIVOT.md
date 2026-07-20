# The Pivot — From "GLP-1 Companion" to a 3-Year Vision

> Synthesizes everything researched so far into a sharper, more aspirational concept. Responds to
> two pieces of feedback: (1) a pure GLP-1 adherence-coach framing is generic — expect several
> competing teams to pitch some version of it at this exact hackathon; (2) a judge explicitly asked
> for a 3-year-out, aspirational lens. Builds on [ARCHITECTURE-DECISIONS.md](./ARCHITECTURE-DECISIONS.md),
> [DISEASE-LANDSCAPE.md](./DISEASE-LANDSCAPE.md), and [CRITICAL-APPRAISAL-KIDNEY.md](./CRITICAL-APPRAISAL-KIDNEY.md).

---

## The reframe in one line

**Stop pitching "a coaching app for GLP-1 patients." Start pitching "voice as the free, continuous
biomarker that catches metabolic disease progressing toward organ damage — years before a
diagnosis would."** Same underlying build. Completely different identity.

---

## Why this solves both problems at once

**"Too generic" solved:** the product is no longer defined by *which drug the patient is on*. A
GLP-1 adherence app is a crowded, obvious category at *this specific hackathon* — eMed's sponsor,
weight-loss-drug hype, half the room will pitch some version of it. An **organ-trajectory
early-warning system** is a category of one. GLP-1/obesity is just the *first population you'd
deploy it in*, not the identity of the company.

**"Aspirational, 3 years out" solved:** you asked for this and it's a genuinely honest fit, not a
stretch. Everything researched so far is exactly the evidence base for it:

| Signal (already researched) | Organ/system it watches | Maturity today |
|---|---|---|
| Frailty/sarcopenia markers (A1/A2 zero-crossing, peak variation) | **Muscle** | 2024–25 published, buildable now |
| Prosody / affect | **Mood / adherence risk** | Mature, buildable now |
| Glycemic voice markers (jitter/shimmer via laryngeal tissue elasticity) | **Metabolic control** | Early but real, 2023–24 studies |
| Congestion markers (jitter, HNR, formants) | **Cardiac (HFpEF risk)** | Strong (HearO), contested — Cordio owns HF directly |
| Fluid/Reinke's-space markers | **Renal** | Real mechanism, weak/old evidence, hostile market ([why](./CRITICAL-APPRAISAL-KIDNEY.md)) |

**The insight the pitch is built on:** obesity, prediabetes, T2D, cardiac strain, and kidney
involvement aren't separate diseases requiring separate products — they're **stages of one
progression**, and the same physiological substrate (systemic inflammation, fluid handling, tissue
composition) that drives the disease also measurably touches the voice at every stage. Nobody
today is building **one listening layer that follows a patient across that entire trajectory.**
Point-solution companies (Cordio for HF, hypothetical others for CKD) each grab one stage. That gap
— not any single organ — is the actual white space, and it's a much bigger, more defensible claim
than "we made HearO's trick apply to a new organ."

---

## What you'd actually say to the judges (the aspirational beat)

*"Today, we're going to show you one working piece: a voice check-in that catches early muscle
loss in a GLP-1 patient — years before a DEXA scan would. But that's not the product. The product
is the listening layer underneath it. The same architecture that watches muscle today can watch
glycemic control tomorrow, cardiac strain the day after, because we didn't hard-code a model per
disease — we built a reasoning engine [Vadalog/Prometheux] that adds a new organ system by adding
new rules, not by retraining a black box or building a new company. In three years, this doesn't
live inside one drug's coaching app. It lives inside every phone call a chronic-disease patient
already has — with their pharmacy, their insurer, their care team — quietly building a
multi-year picture of which organ system is starting to bear the cost of their condition, long
before a symptom, a hospitalization, or a diagnosis forces the issue. Voice becomes the vital sign
nobody has to remember to take."*

This directly answers "what could AI be in 3 years" with something concrete and demoable *today*,
not vague futurism — which is exactly what makes an aspirational pitch land instead of drifting
into sci-fi.

---

## What actually changes in the build (not much — this is a narrative shift, not a rebuild)

- **Keep:** the voice check-in architecture, the escalation/reasoning layer (Vadalog rule-based,
  explainable), the muscle/frailty signal as the one thing you build and demo live (still the best
  choice per [ARCHITECTURE-DECISIONS.md](./ARCHITECTURE-DECISIONS.md) — current evidence, no
  competing measurement, fits the population).
- **Change:** the identity on the slide. It's not "[Product] — your GLP-1 companion." It's
  "[Product] — a listening layer that tracks your metabolic disease trajectory before your organs
  do." The GLP-1/obesity persona stays as the demo's *entry point*, not its ceiling.
- **Add one slide beat:** a simple visual showing the progression (obesity → prediabetes/T2D →
  cardiac/renal involvement) with your four researched signals mapped onto it — this *is* your
  3-year roadmap slide, and you already have the citations for every node on it.
- **Say the honest part out loud:** name which signal is real and built today (muscle/frailty),
  and which are roadmap (cardiac, glycemic, renal) — that honesty is what makes the aspirational
  claim credible instead of hand-wavy, and it's consistent with how you've framed every other claim
  in this research so far.

---

## One naming/identity note

Consider dropping "companion" from the name entirely — it undersells the reframe. Something that
signals *early detection across a trajectory* rather than *coaching* fits better: think in the
direction of "signal," "trajectory," "undertone," "vantage," "prelude" — a name that could
plausibly appear on a monitoring/diagnostics company's door, not a wellness app's.

---

## The honest risk of this reframe

Judges weight **Demo quality** and **Feasibility** heavily, and a platform/vision pitch can drift
abstract if you're not careful. The fix is structural, not rhetorical: **the live demo stays
exactly as concrete and single-patient as before** (Talk → Snap → Hear+escalate, one persona). The
*only* thing that changes is the frame you put around it at the start and the close. Don't build
four signals to prove the platform claim — build one well, and let the roadmap slide (with its
citations) carry the aspirational weight instead.
