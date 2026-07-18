# Prometheux Prompt — Undertone Ontology & Data Flow

Paste this into Prometheux (via the chat/project interface, or feed it to `create_project` +
`create_concept` calls) to build a real, inspectable ontology that mirrors exactly what the demo
does. The goal for the hackathon: a judge can look at the resulting graph and see that the
"explainable escalation" claim in the demo isn't decorative — there's an actual reasoning model
behind it, expressed as auditable rules over structured facts, not a black-box prompt.

---

## The prompt

```
Build a Vadalog ontology called "Undertone" that models a voice-based early-warning system for
chronic metabolic disease. The purpose of this ontology is to make an AI clinical escalation
fully explainable and auditable — every alert must be traceable to the exact facts and rules that
produced it, with no black-box scoring.

DOMAIN CONTEXT
Patients on a chronic metabolic programme (GLP-1 / obesity / T2D) do a daily 60-second voice
check-in. Acoustic features are extracted from the recording. Over time, drift in those features
signals early organ-system decline — starting with muscle loss (sarcopenia), with cardiac,
glycemic, and renal signals as a roadmap. When drift crosses a clinically-grounded threshold, and
especially when it contradicts what the patient self-reports, the system escalates to a human
clinician with a full reasoning trace. Nothing is auto-diagnosed. Every access and action is
logged for audit.

CONCEPTS (entities) — define each with its key attributes:
1. Patient(patient_id, name, age, programme, consent_scope)
2. VoiceCheckIn(checkin_id, patient_id, timestamp, self_report_text, self_report_sentiment)
3. AcousticFeatures(checkin_id, energy, zero_crossing_rate, pitch_hz, jitter, shimmer, hnr)
4. MuscleIntegrityIndex(patient_id, date, index_value)  — derived from AcousticFeatures
5. Trend(patient_id, window_days, slope, direction)      — derived from MuscleIntegrityIndex history
6. ClinicalEvidence(citation_id, source, finding, url)   — e.g. JMIR 2024 sarcopenia study, HearO study
7. EscalationRule(rule_id, name, condition_description, cited_evidence)
8. Escalation(escalation_id, patient_id, rule_id, priority, reasoning_trace, status, created_at)
9. Clinician(clinician_id, name, role)
10. ConsentGrant(patient_id, scope, granted)
11. AuditLogEntry(entry_id, actor, action, object, timestamp)

RELATIONSHIPS (data flow — model each as a Vadalog derivation rule, not a stored fact, so the
provenance chain is inspectable end to end):

a) AcousticFeatures are derived per VoiceCheckIn (upstream, fed in as facts from the voice/vision
   AI layer — this is the only non-symbolic step; everything downstream is logic).
b) MuscleIntegrityIndex(patient, date, value) is derived by combining AcousticFeatures for that
   date via a weighted rule referencing zero_crossing_rate and shimmer as sarcopenic-frailty
   markers (cite: JMIR 2024 frailty phenotype study).
c) Trend(patient, 18, slope, direction) is derived recursively over the last 18 days of
   MuscleIntegrityIndex facts for that patient.
d) Contradiction(patient, checkin_id) holds when self_report_sentiment = "positive" AND the
   same-day Trend.direction = "rising" AND MuscleIntegrityIndex.value >= threshold(58).
e) EscalationRule "muscle_decline_alert" fires Escalation(patient, rule, priority: high,
   reasoning_trace, status: open) when Trend.slope > 0 AND MuscleIntegrityIndex.value >= 58 AND
   Contradiction holds. The reasoning_trace must be generated as a literal derivation path
   (which facts, which rule, which citation) — not free text from an LLM.
f) Escalation only routes to a Clinician if ConsentGrant(patient, "care_team", true) holds —
   model consent as a hard join condition, not a UI-layer check.
g) Every step in (a)–(f), plus every Clinician read of a Patient record, writes an
   AuditLogEntry(actor, action, object, timestamp) — model logging as a first-class relation
   derived alongside every rule firing, not bolted on afterward.
h) Model the roadmap explicitly: define CardiacSignal, GlycemicSignal, RenalSignal as concept
   stubs with the same shape as MuscleIntegrityIndex (derived-value + Trend + threshold rule),
   currently unpopulated, to show the ontology extends by adding rules, not by retraining a model.

OUTPUT REQUESTED
1. A visual ontology graph: entities as nodes, derivation rules as labeled directed edges, clearly
   separating "raw/ingested facts" (Patient, VoiceCheckIn, AcousticFeatures, ClinicalEvidence,
   ConsentGrant) from "derived/reasoned facts" (MuscleIntegrityIndex, Trend, Contradiction,
   Escalation, AuditLogEntry) — use visually distinct styling for the two layers.
2. A data-flow diagram showing the pipeline stage by stage: Voice/Vision capture (non-symbolic) →
   Feature extraction (non-symbolic) → Vadalog reasoning layer (symbolic, everything from here is
   auditable) → Consent gate → Clinician escalation → Audit log.
3. For the muscle_decline_alert rule specifically, render the full derivation trace for one
   example patient (Sarah, index=61, slope positive over 18 days, self-report "fine") so it can be
   screenshotted directly into a pitch deck slide as proof the reasoning is real, not illustrative.
4. Keep concept and rule names exactly as given above so the output maps 1:1 onto the working
   product demo (same rule name, same fields) — the ontology should look like the specification
   the demo was built from, not a separate mock.
```

---

## Why this prompt is built this way

- **Names match the live demo exactly** (`muscle_decline_alert`, the same fields, the same
  threshold of 58, the same Sarah persona) — so the Prometheux output and the running app read as
  one system to a judge, not two disconnected artifacts.
- **Explicitly separates ingested facts from derived facts** — this is the single clearest way to
  show a technical judge *why* this is more defensible than "we asked an LLM to explain itself":
  the derived layer is symbolic and traceable by construction.
- **Bakes in the citations** (JMIR 2024, HearO) as first-class ontology objects, not comments —
  so "evidence-based" is a property of the data model, not just a claim in the pitch.
- **Models consent and audit as ontology relations**, not app-layer afterthoughts — directly
  answers the compliance concern raised earlier in this project (no ungated cross-patient access,
  every action logged) at the architecture level, which is a stronger signal to judges than a UI
  badge alone.
- **Includes the roadmap organs as concept stubs** — gives Prometheux something concrete to
  diagram for the "3-year vision" slide, reinforcing that scaling to new organ systems is a rules
  change, not a rebuild.

## How to use the output
- Screenshot the ontology graph and the single-patient derivation trace → drop them into
  **Slide 4 ("Why it's defensible")** of the Gamma deck ([GAMMA-DECK-PROMPT.md](./GAMMA-DECK-PROMPT.md)) alongside the HearO comparison.
- If Prometheux's MCP tools are reachable in your environment, this same prompt can be split into
  sequential `create_concept` calls (one per numbered concept) followed by relationship/rule
  definitions and a `save_ontology` — the prose above is written so it transcribes directly into
  that structure. When I attempted this live during the session, the Prometheux MCP connection
  timed out three times, so this document is the ready-to-paste fallback; retry the tool calls
  yourself if the connection is back up.
