# ReimagineHealth — Winning Strategy

> Companion to [HACKATHON.md](./HACKATHON.md). Built on the hackathon-ai-strategist
> framework, adapted to this specific event: ~20 hours, overnight, team of up to 4,
> OpenAI as the headline sponsor, demo quality explicitly weighted above slides.

---

## 0. The one thing that wins this

eMed's whole pitch is **adherence: 90% vs an industry 40–60%**. That is the number the
judges (clinical + eMed + OpenAI + VCs from Eka/Dawn) care about. So the winning
submission is not "a health app." It is **a believable answer to: why would a patient
actually keep doing this for 6 months — and how does AI catch the thing a human would
miss between appointments?**

Every idea below is filtered against that. If a concept doesn't obviously move adherence
or catch a missed signal, it loses on *User impact* no matter how clever.

---

## 1. Context to confirm at kickoff (do this first, 15 min)

The strategist rule: don't design before these are answered. Find an eMed mentor Friday
7pm and ask:

1. **What OpenAI credits/APIs are provided?** (Realtime voice API? GPT-4o vision? Assume yes, confirm.)
2. **Is there any sample patient data / biomarker feed?** (CGM, blood pressure, weight logs — a realistic dataset saves hours.)
3. **What are they tired of seeing?** (Judges reward ideas that reframe; ask directly.)
4. **Any prize sub-tracks** (e.g. an OpenAI "best use of Realtime API" prize) to aim at.
5. **Submission format** — repo + video? Live demo slot length?

These answers can flip the ranking below. Lock them before designing.

---

## 2. How to find the best idea (the method, not just the answer)

Run this in the first 60–90 minutes, then **stop and commit**. Indecision is the #1
killer at a 20h hackathon.

**Step 1 — Diverge (20 min).** Brainstorm against the 5 challenge prompts. Aim for ~10 raw
ideas. No filtering yet.

**Step 2 — Three hard gates.** Kill any idea that fails even one:
- **Demoable-in-60s gate:** Can a judge *see* the magic happen live in under a minute? (Demo quality is weighted — anything that only works "at scale" or "over 6 weeks" fails here unless you can fake the timeline convincingly.)
- **Post-2023-capability gate:** Does it rely on something that genuinely couldn't have been built two years ago? (Realtime speech-to-speech, multimodal vision reasoning, agentic tool use.) This is how you win *Innovation*.
- **Adherence-or-signal gate:** Does it plausibly raise adherence or catch a missed clinical signal? This is how you win *User impact*.

**Step 3 — Score the survivors** 1–5 on each judging axis, then weight them. Suggested
weights matching this event's stated criteria: User impact 30, Innovation 30,
Feasibility 20, Demo 20. Pick the **highest weighted total**, not the most exciting idea.

**Step 4 — Two gut checks:**
- *"Would eMed ship this Monday?"* (Feasibility.)
- *"Does the demo have one moment that makes the room go quiet?"* (Demo.)

---

## 3. Three ranked concepts

### 🥇 #1 — "Companion" : a proactive AI voice coach that runs the programme between appointments
- **Problem (one sentence):** Patients on 6-month chronic-care programmes drift, skip meds, and hide deterioration because nobody checks in until the next appointment — that's where the 40–60% adherence goes to die.
- **AI mechanism:** OpenAI **Realtime API** (speech-to-speech) voice agent with persistent memory of the patient's history. It does a short daily spoken check-in that *feels human*, remembers yesterday, notices what's off in natural conversation, nudges adherence, and — via function calling — **auto-drafts a structured red-flag summary to the clinical team** when it spots a pattern.
- **Riskiest assumption:** Live voice latency/reliability on stage.
- **Fallback:** Pre-scripted push-to-talk demo, or a pre-recorded voice clip triggered live; the memory + escalation logic still demos on screen.
- **OpenAI fit:** 3/3 — Realtime API is the headline sponsor tech and <2yr old.
- **Why it wins:** Hits every axis. "Feels human" is literally in the brief. The demo moment — *talking to it live, it recalls yesterday and escalates to your nurse* — is the room-goes-quiet moment.

### 🥈 #2 — "Snap" : frictionless logging that kills the #1 adherence killer
- **Problem:** Manual logging (meals, glucose, BP, weight) is the friction that makes patients quit; missed logs = missed signals.
- **AI mechanism:** GPT-4o **vision** — photograph a meal / glucose monitor / BP cuff / medication, it returns structured data + plain-language advice + a trend alert. Zero typing.
- **Riskiest assumption:** Vision misreads numbers off a device screen.
- **Fallback:** Curated demo images known to work; still shows the flow.
- **OpenAI fit:** 3/3.
- **Why #2 not #1:** Lower demo risk and very buildable, but slightly less "wow" and less novel than a live voice agent. Excellent as a *layer inside #1* rather than a standalone.

### 🥉 #3 — "Bridge" : agentic triage between patient and clinician
- **Problem:** Clinicians can't watch every patient's home data stream; patients need constant appointments to feel safe.
- **AI mechanism:** An agent continuously reasons over home data (CGM/BP/weight/symptoms), and *only* surfaces to a human clinician when patterns warrant — with a drafted summary. "Connect patients to clinical teams without constant appointments," straight from the brief.
- **Riskiest assumption:** Needs believable streaming data + a clinician-side UI to be legible.
- **Fallback:** Seeded dataset + simulated "time passing."
- **OpenAI fit:** 2/3 — agentic, less visually tied to a flagship API.
- **Why #3:** Strongest "real-world deployable" story, weakest live-demo wow. Its escalation idea is best absorbed into #1.

---

## 4. Recommendation

**Build #1 (Companion), with the vision layer from #2 as the single "wow" add-on, and #3's
escalation as the closing beat.** One patient persona, one hero flow, three beats:

1. **Talk to it** (Realtime voice) → it recalls yesterday, feels human. *(Innovation + Impact)*
2. **Snap a reading** (vision) → high BP photo logged instantly, no typing. *(Impact + friction story)*
3. **It escalates** → drafts a red-flag summary to the nurse dashboard. *(Feasibility + the "eMed ships Monday" story)*

That's a coherent 3-minute pitch that touches all four judging axes and demos live.

---

## 5. Execution timeline (20h, overnight — adapted phases)

Team of 4 shift-sleeps; effective build time is ~12–14h, so **scope is one hero flow only.**

| Time | Phase | Goal |
|---|---|---|
| Fri 7:00–8:00pm | **Ideation + kickoff intel** | Confirm §1 questions, run §2 method, **lock the concept by 8:00pm.** |
| 8:00–9:30pm | **Setup + spike** | Repo, deploy target, and a 30-min spike on the riskiest bit: get one Realtime API round-trip working. If it won't work, pivot to #2 now. |
| 9:30pm–1:00am | **Core build** | Voice loop + patient memory (seeded persona) end-to-end. Get the happy path talking. |
| 1:00–5:00am | **Shift sleep + layer** | 2 sleep while 2 add the vision snap + escalation write. Rotate. |
| 5:00–9:00am | **Integrate + seed** | Wire the 3 beats into one flow. Seed "Sarah" with realistic 6-week history. |
| 9:00–11:00am | **Demo hardening** | Error-handle the 3 likely break points. **Record a backup screen capture of the full demo.** |
| 11:00am–1:00pm | **Freeze + polish** | Scope freeze. Anything not on the demo path is cut. Light UI polish only. |
| 1:00–2:30pm | **Pitch + rehearse** | Build the 6-slide deck, rehearse the 3-min pitch twice on the actual presentation laptop. |
| 2:30–3:00pm | **Submit + buffer** | Submit early. Buffer for chaos. |

**Go/No-Go gates:** (a) 8pm — concept locked and one-person-buildable? (b) 9:30pm — does the
Realtime round-trip work? If no → pivot to #2. (c) ~9am — is the happy path stable? If no →
freeze to what exists and make *that* demo flawless.

---

## 6. Architecture (senior-engineer plan, kept lean)

Optimize for "fewest moving parts that still wow." Suggested stack:

- **Frontend:** Next.js + Tailwind, one page + a "clinician" tab. Deploy on **Vercel**.
- **Voice:** OpenAI **Realtime API** over WebRTC (their sample gets you 80% there — start from it, don't build from scratch).
- **Vision:** GPT-4o vision endpoint for the snap-to-log beat.
- **Backend/memory:** **Supabase** (Postgres) — one `patient`, one `checkins`, one
  `alerts` table. Seed it. (You already have Supabase tooling wired up locally.)
- **Escalation:** the voice agent uses **function calling** to insert an `alerts` row; the
  clinician tab reads it live.
- **UI speed:** use a design generator (e.g. Stitch, which you have available) for the shell
  so an engineer isn't hand-crafting CSS at 3am.

**Build-depth rule:** the voice loop and the escalation write are built for *real*. Everything
else (the 6-week history, the "programme," multiple patients) is **seeded/mocked** — it only
has to look real in the demo.

---

## 7. Demo strategy (this is weighted — treat it as a deliverable, not an afterthought)

**One persona:** "Sarah, 52, 6 weeks into a GLP-1 + lifestyle programme for T2D + obesity."
Seed her history so every recall the agent makes lands.

**3-minute pitch skeleton:**
| Segment | Time | Content |
|---|---|---|
| Hook | 30s | "40–60% of chronic-care patients quit their programme. The ones who don't? Somebody was paying attention between appointments. We built that somebody." |
| Solution | 30s | What Companion is + the Realtime/vision/agent mechanism in one breath. |
| Live demo | 60s | The 3 beats: talk → snap → escalate. Narrate each. |
| Architecture | 20s | One diagram: Realtime API + vision + Supabase memory + clinician escalation. |
| Impact | 20s | Tie to adherence; one growth vector (conditions/geographies). |
| Team + ask | 20s | Who built it; what a month would add. |

**Reliability checklist (before the room):**
- [ ] Full-demo screen recording as backup — switch to it without apology if live voice fails.
- [ ] Demo account seeded with realistic, non-placeholder data.
- [ ] Rehearsed twice **on the presentation laptop**, on the venue network.
- [ ] Vision demo uses known-good images.
- [ ] Notifications off, unrelated tabs closed, mic tested.
- [ ] A one-line answer ready for each likely judge question (see below).

**Likely judge questions — have answers ready:**
- *"Is this clinically safe / who's liable?"* → It escalates to a human clinician; it never diagnoses. It's a triage/adherence layer, not a doctor.
- *"How is this different from a chatbot?"* → Persistent memory + proactive check-ins + it acts (escalates), and it's voice-first because voice is what feels human and gets used daily.
- *"Would this actually raise adherence?"* → Daily human-feeling contact + zero-friction logging attack the two biggest drop-off causes directly.

---

## 8. Common failure modes to avoid

- **Over-scoping.** Four beats is too many; three is the max. Cut ruthlessly at 11am.
- **Building infra nobody sees.** Auth, multi-user, real device integrations — all mocked.
- **A demo that needs narration to make sense.** If the magic isn't visible, it doesn't score.
- **Leaving the pitch to the last 20 minutes.** Demo quality is weighted — rehearse twice.
- **Live-voice hubris.** Always have the recorded backup. Always.
