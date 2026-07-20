# Undertone — Cue Cards (3-min pitch + demo)

**Product:** Undertone — the listening layer. *(strategist's alt name: "Cadence" — swap the word in `demo/index.html` header if preferred.)*
**Run demo:** preview server `undertone-demo` on port 4599. One persona: Sarah, 52, T2D + obesity, week 6.
**Demo path:** Patient tab → tap mic → speak → analysis shows drift + contradiction → Clinician tab shows explainable escalation. (Have the pre-recorded screen capture ready as backup.)

---

## 3-minute pitch (time-annotated)

**[0:00–0:25] Hook**
> "2 million people in the UK are on GLP-1s. Every one is told they'll lose weight. What they're *not* told is that up to 60% of it can be muscle — and the only tool that catches it is a DEXA scan almost nobody gets until the damage is done. We built the tool that catches it for free, every day, using something you already have: your voice."

**[0:25–0:55] Solution**
> "This is Undertone — a voice-based listening layer for chronic metabolic disease. Every day the patient does a 60-second voice check-in with an AI companion. Underneath that conversation we extract acoustic biomarkers — the same class of signal already shown to detect heart failure 3 weeks before hospitalization, and sarcopenia in peer-reviewed 2024–25 studies. We track drift per organ system, and when it crosses a threshold, an explainable rule-based engine escalates a cited summary straight to the clinician."

**[0:55–1:55] Live demo** *(see narration below)*

**[1:55–2:15] Architecture**
> "Three layers: voice + vision intake on OpenAI's realtime and vision APIs; a biomarker layer turning raw audio into longitudinal features per organ; and an explainable escalation engine — no black box, every flag is a rule with a citation a clinician can trust in seconds."

**[2:15–2:35] Impact**
> "Daily-weight monitoring catches deterioration at 20% sensitivity. Voice, in published cardiac studies, hits 76–81%, three weeks earlier — from something patients already do: talk. Today it's muscle loss. The same architecture is scoped for cardiac, glycemic, and renal — one listening layer, four organ systems, zero hardware."

**[2:35–3:00] Aspirational close**
> "We built the muscle detector live this weekend to prove it works. In three years, we believe the annual check-up disappears. Disease isn't caught at a scheduled appointment once a year — it's caught the Tuesday it starts, because your voice already told us, before you knew yourself."

---

## Demo narration (3 beats)

**Beat 1 — Talk** *(Patient tab, tap mic, speak)*
> "This is Sarah, day 21 on a GLP-1 programme. She does her daily check-in — notice it already remembers yesterday, that her legs felt heavy. No app to open, no form. Just talking. And while she talks, we're extracting acoustic features on-device — live, right here."

**Beat 2 — Snap** *(tap the camera)*
> "She snaps her weigh-in. Vision reads the number straight off, logs it against her trend — zero typing, so the data's actually reliable."

**Beat 3 — Hear + Escalate** *(analysis lands, then Clinician tab)*
> "But here's the real product. Under every check-in, her muscle-integrity voice index has been drifting for 18 days — and today it crossed the line. She *told* us she feels fine. Her voice disagreed. That contradiction is the signal. And it doesn't fire a vague alert — it escalates a cited, structured reasoning trace to her clinician: what drifted, by how much, and why. Talk, snap — and the system is already watching for the thing nobody else is looking for."

---

## Tough judge questions → one-line answers

1. **"How do you separate drift from a cold / bad mic day?"** → We never act on one reading — only sustained multi-day drift against the patient's own baseline, exactly like HearO's model, not single-sample noise.
2. **"Where's the clinical validation?"** → We say it plainly: this is a triage signal, not a diagnostic — built on published sensitivity data (HearO 76–81%, n=545; JMIR sarcopenia studies), sitting *upstream* of DEXA/echo, not replacing them.
3. **"Why would a clinician trust it, not ignore it?"** → It's not a black-box score — every escalation is a cited, rule-based explanation, so the clinician sees the evidence, not just a red flag. That's the whole design.

---

## Reliability checklist
- [ ] Pre-recorded screen capture of the full flow (backup if live mic fails — the app also auto-falls-back to sample audio if mic is denied).
- [ ] Grant mic permission before presenting; test in a quiet spot.
- [ ] Rehearse twice on the presentation laptop; browser notifications off.
- [ ] Open on the Patient tab, zoomed so text is legible from the back.
