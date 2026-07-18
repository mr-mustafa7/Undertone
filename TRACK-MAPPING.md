# Track Mapping — How the Concept Hits Every Assessed Area

> Maps the fused concept — **a proactive AI voice companion for chronic-condition patients that
> passively hears vocal biomarkers, interprets home data, and escalates to clinicians** — onto the
> five challenge areas in [HACKATHON.md](./HACKATHON.md). Built on
> [VOCAL-BIOMARKER-SCIENCE.md](./VOCAL-BIOMARKER-SCIENCE.md) and [IDEA-DEEPDIVE.md](./IDEA-DEEPDIVE.md).

**The headline:** most teams will pick one challenge bullet. This concept credibly answers **all
five**, because "a daily voice conversation that doubles as a passive sensor and a clinician bridge"
naturally spans monitoring, support, complex conditions, care connection, and data interpretation.
That breadth is the differentiator — *if* pitched as one narrative, not a checklist.

---

## The 1:1 mapping

### Track 1 — "Inadequate at-home biomarker monitoring → immature data interpretation, missed critical signals"
**This is the bullseye.** Today's home signal for heart failure — daily weight — has only ~20%
sensitivity and moves too late, so critical deterioration is *missed*. Vocal biomarkers catch
congestion at **76–81% sensitivity, ~3 weeks earlier** (Cordio HearO, 545 patients).
- **Build:** real openSMILE/parselmouth feature extraction (jitter, shimmer, HNR, intensity) on the
  companion's daily audio; drift detection vs the patient's personal baseline.
- **Demo beat:** the "hear + escalate" moment — a signal weight monitoring would have missed.
- **Pitch line:** *"The scale catches 1 in 5 events, too late. The voice catches 4 in 5, three weeks early."*

### Track 2 — "AI that offers genuine, ongoing support for long-term health"
The companion is **proactive** (it reaches out — the fix for the 2%-sustained-use problem) and
**persistent** (remembers yesterday), delivering the *frequency of contact* clinically proven to
keep patients on programme (5+ contacts → 49× persistence).
- **Build:** OpenAI Realtime voice + patient memory; check-ins tied to medication/symptom moments.
- **Demo beat:** the opening "Talk" — it calls, remembers her nausea from yesterday, feels human.
- **Pitch line:** *"Support that shows up daily, remembers you, and never needs you to open an app."*

### Track 3 — "Better support for complex needs like heart or women's health"
Heart failure is the exemplar complex, high-stakes condition — and the vocal biomarker is
*specifically* a cardiac early-warning. The same passive-voice approach extends to women's health
(the voice-biomarker field already covers it), giving a clear expansion story.
- **Build:** anchor the demo on an HF/metabolic persona; note the generalization.
- **Demo beat:** framing the persona as managing heart + metabolic risk together.
- **Pitch line:** *"Built for the conditions where a missed signal means a hospital bed."*

### Track 4 — "Connect patients with clinical teams without constant real-time appointments"
The escalation layer is exactly this: the AI runs the between-visit middle and only pulls in a human
clinician **when a threshold is actually crossed**, with a **drafted summary** so the clinician's
time is spent on decisions, not data-gathering. Solves the specialist-shortage bottleneck.
- **Build:** function-calling writes a structured `alert` → a simple clinician dashboard tab.
- **Demo beat:** the nurse's screen lighting up with the drafted red-flag summary.
- **Pitch line:** *"Your care team, on-demand — reached automatically the moment it matters, not before."*

### Track 5 — "Turn complex health data into simple, actionable advice"
Raw jitter/HNR/BP/glucose is meaningless to a patient (and causes anxiety). The companion speaks it
back as plain, personalized, *actionable* language.
- **Build:** LLM turns the feature/vital trends into a spoken plain-language read + one next action.
- **Demo beat:** the "Snap" reading → "Your BP's up a little; let's cut the salt tonight and I'll
  recheck tomorrow."
- **Pitch line:** *"No numbers to decode — just what's happening and what to do about it."*

---

## How to PITCH it so the mapping lands (without sounding like a checklist)

Don't say "we hit all five tracks." Tell **one story** whose beats *happen* to be the five tracks.
Anchor on the vivid problem (Track 1, missed signals) and close on the bridge (Track 4).

**The 3-minute spine:**
1. **Hook (Track 1):** "Every year, patients are hospitalized for heart failure that was building
   for weeks — but the scale, our main home tool, catches only 1 in 5, and too late." *(missed
   critical signals)*
2. **Insight (Track 3 + why-now):** "The warning was there the whole time — in their voice.
   Congestion changes how you sound days before you feel it, and AI can now hear it." *(complex
   condition + novel capability)*
3. **Product (Track 2):** "So we built a companion that calls every day — not another app to
   forget. It supports the programme, remembers you, keeps you adherent." *(ongoing support)*
4. **Live demo (Tracks 5 + 1):** Talk → Snap (plain-language data read, Track 5) → Hear (the voice
   biomarker drifts past threshold, Track 1). *(actionable advice + monitoring)*
5. **Bridge (Track 4):** "And the moment it matters, it drafts a summary to the nurse — care team
   connection without a single extra appointment." *(clinical connection)*
6. **Impact + ask:** adherence ROI (49×) + readmission ROI (voice ~4× weight's sensitivity) + the
   data-flywheel moat.

**Why this wins the assessed criteria too** (the four judging axes, distinct from the tracks):
- **User impact:** touches adherence *and* early-warning — two of the biggest cost/mortality levers.
- **Innovation:** passive vocal biomarker inside a conversational agent = the freshest thing in the room.
- **Feasibility:** everything but the biomarker labels is built for real; honest framing on that one beat.
- **Demo quality:** three visible, narratable beats with a genuine "wow" — exactly what "a live demo
  matters more than slides" rewards.

---

## One strategic caution

Breadth is a strength *only* if the demo stays tight. Build **one persona, three beats** (Talk →
Snap → Hear+Escalate). Each beat quietly covers 1–2 tracks. Do **not** try to build a separate
feature per track — that's the over-scoping trap. Let the *narrative* show breadth while the *build*
stays narrow.
