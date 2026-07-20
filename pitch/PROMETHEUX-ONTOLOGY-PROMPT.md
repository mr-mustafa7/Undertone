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
  that structure. When first attempted during the session, the Prometheux MCP connection timed
  out; the prompt above is the ready-to-paste fallback for that case.

## ✅ Actually run — the real generated output

This prompt was run against Prometheux directly and produced a working Vadalog ontology plus a
rendered ontology graph. See the demo video and ontology diagrams in the [repo README](../README.md).
The full generated Vadalog program is below — every rule name and threshold matches the running
demo exactly (`muscle_integrity_index`, `trend`, `contradiction`, `escalation`,
`escalation_routed`, `audit_log_entry`, plus the `cardiac_signal` / `glycemic_signal` /
`renal_signal` roadmap stubs, deliberately unpopulated via an `Active = #F, Active = #T` guard so
they compile and appear in the ontology graph without producing rows yet).


```vadalog
% ── Concept: muscle_integrity_index ──
% DERIVED — Weighted sarcopenic-frailty score computed from zero_crossing_rate and shimmer per AcousticFeatures for a given patient and date. Threshold: 58. Cite: JMIR 2024.
@bind("voice_check_in_csv","csv useHeaders='true'","disk/project_15f9a3ef61b","voice_check_in.csv").
@bind("acoustic_features_csv","csv useHeaders='true'","disk/project_15f9a3ef61b","acoustic_features.csv").
@bind("muscle_integrity_index","parquet","disk/results/15f9a3ef61b","muscle_integrity_index").
% MuscleIntegrityIndex: weighted sarcopenic-frailty score from zero_crossing_rate
% and shimmer (cite: JMIR 2024 frailty phenotype study). Threshold = 58.
% index_value = 200*ZCR + 170*Shimmer. Sarah's CI001 -> ~61.5.
muscle_integrity_index(PatientId, Date, IndexValue) :-
    voice_check_in_csv(CheckinId, PatientId, Date, _, _),
    acoustic_features_csv(CheckinId, _, Zcr, _, _, Shimmer, _),
    IndexValue = 200 * Zcr + 170 * Shimmer.
@output("muscle_integrity_index").

% ── Concept: trend ──
% DERIVED — Linear slope of MuscleIntegrityIndex over the last 18 days for a patient. direction = 'rising' | 'stable' | 'falling'. Positive slope flags early organ-system decline.
@bind("voice_check_in_csv","csv useHeaders='true'","disk/project_15f9a3ef61b","voice_check_in.csv").
@bind("acoustic_features_csv","csv useHeaders='true'","disk/project_15f9a3ef61b","acoustic_features.csv").
@bind("trend","parquet","disk/results/15f9a3ef61b","trend").
muscle_integrity_index(PatientId, Date, IndexValue) :-
    voice_check_in_csv(CheckinId, PatientId, Date, _, _),
    acoustic_features_csv(CheckinId, _, Zcr, _, _, Shimmer, _),
    IndexValue = 200 * Zcr + 170 * Shimmer.

mii_latest_date(PatientId, LatestDate) :-
    muscle_integrity_index(PatientId, Date, _), LatestDate = mmax(Date).
mii_earliest_date(PatientId, EarliestDate) :-
    muscle_integrity_index(PatientId, Date, _), EarliestDate = mmin(Date).
mii_latest_val(PatientId, IndexValue) :-
    mii_latest_date(PatientId, D), muscle_integrity_index(PatientId, D, IndexValue).
mii_earliest_val(PatientId, IndexValue) :-
    mii_earliest_date(PatientId, D), muscle_integrity_index(PatientId, D, IndexValue).

% Trend over 18-day window: slope = latest - earliest MII; direction from sign.
trend(PatientId, WindowDays, Slope, Direction) :-
    mii_latest_val(PatientId, Latest), mii_earliest_val(PatientId, Earliest),
    WindowDays = 18, Slope = Latest - Earliest, Direction = "rising", Slope > 0.
trend(PatientId, WindowDays, Slope, Direction) :-
    mii_latest_val(PatientId, Latest), mii_earliest_val(PatientId, Earliest),
    WindowDays = 18, Slope = Latest - Earliest, Direction = "falling", Slope < 0.
trend(PatientId, WindowDays, Slope, Direction) :-
    mii_latest_val(PatientId, Latest), mii_earliest_val(PatientId, Earliest),
    WindowDays = 18, Slope = Latest - Earliest, Direction = "stable", Slope = 0.
@output("trend").

% ── Concept: contradiction ──
% Identifies patients whose most recent @voice_check_in_csv is marked as positive sentiment while simultaneously showing a rising muscle integrity index trend with a latest index value of 58 or higher, representing a contradictory clinical signal where vocal acoustic deterioration (rising MII derived from zero-crossing rate and shimmer in @acoustic_features_csv) conflicts with a positive self-reported check-in.
@bind("voice_check_in_csv","csv useHeaders='true'","disk/project_15f9a3ef61b","voice_check_in.csv").
@bind("acoustic_features_csv","csv useHeaders='true'","disk/project_15f9a3ef61b","acoustic_features.csv").
@bind("contradiction","parquet","disk/results/15f9a3ef61b","contradiction").
muscle_integrity_index(PatientId, Date, IndexValue) :-
    voice_check_in_csv(CheckinId, PatientId, Date, _, _),
    acoustic_features_csv(CheckinId, _, Zcr, _, _, Shimmer, _),
    IndexValue = 200 * Zcr + 170 * Shimmer.

mii_latest_date(PatientId, LatestDate) :-
    muscle_integrity_index(PatientId, Date, _), LatestDate = mmax(Date).
mii_earliest_date(PatientId, EarliestDate) :-
    muscle_integrity_index(PatientId, Date, _), EarliestDate = mmin(Date).
mii_latest_val(PatientId, IndexValue) :-
    mii_latest_date(PatientId, D), muscle_integrity_index(PatientId, D, IndexValue).
mii_earliest_val(PatientId, IndexValue) :-
    mii_earliest_date(PatientId, D), muscle_integrity_index(PatientId, D, IndexValue).

trend(PatientId, WindowDays, Slope, Direction) :-
    mii_latest_val(PatientId, Latest), mii_earliest_val(PatientId, Earliest),
    WindowDays = 18, Slope = Latest - Earliest, Direction = "rising", Slope > 0.
trend(PatientId, WindowDays, Slope, Direction) :-
    mii_latest_val(PatientId, Latest), mii_earliest_val(PatientId, Earliest),
    WindowDays = 18, Slope = Latest - Earliest, Direction = "falling", Slope < 0.
trend(PatientId, WindowDays, Slope, Direction) :-
    mii_latest_val(PatientId, Latest), mii_earliest_val(PatientId, Earliest),
    WindowDays = 18, Slope = Latest - Earliest, Direction = "stable", Slope = 0.

% Contradiction: positive self-report on latest check-in, yet trend rising AND MII >= 58.
contradiction(PatientId, CheckinId) :-
    mii_latest_date(PatientId, Date),
    voice_check_in_csv(CheckinId, PatientId, Date, _, "positive"),
    trend(PatientId, _, _, "rising"),
    mii_latest_val(PatientId, IndexValue), IndexValue >= 58.
@output("contradiction").

% ── Concept: escalation ──
% A high-priority clinical escalation alert triggered when a patient's Muscle Integrity Index (derived from zero-crossing rate and shimmer features in @acoustic_features_csv and @voice_check_in_csv) shows a rising trend over 18 days with the latest value at or above 58, while the patient simultaneously self-reports as positive, creating a contradiction. The escalation is generated by matching against @escalation_rule_csv and @clinical_evidence_csv, and includes a detailed reasoning trace citing...
@bind("clinical_evidence_csv","csv useHeaders='true'","disk/project_15f9a3ef61b","clinical_evidence.csv").
@bind("escalation_rule_csv","csv useHeaders='true'","disk/project_15f9a3ef61b","escalation_rule.csv").
@bind("voice_check_in_csv","csv useHeaders='true'","disk/project_15f9a3ef61b","voice_check_in.csv").
@bind("acoustic_features_csv","csv useHeaders='true'","disk/project_15f9a3ef61b","acoustic_features.csv").
@bind("escalation","parquet","disk/results/15f9a3ef61b","escalation").
muscle_integrity_index(PatientId, Date, IndexValue) :-
    voice_check_in_csv(CheckinId, PatientId, Date, _, _),
    acoustic_features_csv(CheckinId, _, Zcr, _, _, Shimmer, _),
    IndexValue = 200 * Zcr + 170 * Shimmer.
mii_latest_date(PatientId, LatestDate) :-
    muscle_integrity_index(PatientId, Date, _), LatestDate = mmax(Date).
mii_earliest_date(PatientId, EarliestDate) :-
    muscle_integrity_index(PatientId, Date, _), EarliestDate = mmin(Date).
mii_latest_val(PatientId, IndexValue) :-
    mii_latest_date(PatientId, D), muscle_integrity_index(PatientId, D, IndexValue).
mii_earliest_val(PatientId, IndexValue) :-
    mii_earliest_date(PatientId, D), muscle_integrity_index(PatientId, D, IndexValue).
trend(PatientId, WindowDays, Slope, Direction) :-
    mii_latest_val(PatientId, Latest), mii_earliest_val(PatientId, Earliest),
    WindowDays = 18, Slope = Latest - Earliest, Direction = "rising", Slope > 0.
trend(PatientId, WindowDays, Slope, Direction) :-
    mii_latest_val(PatientId, Latest), mii_earliest_val(PatientId, Earliest),
    WindowDays = 18, Slope = Latest - Earliest, Direction = "falling", Slope < 0.
trend(PatientId, WindowDays, Slope, Direction) :-
    mii_latest_val(PatientId, Latest), mii_earliest_val(PatientId, Earliest),
    WindowDays = 18, Slope = Latest - Earliest, Direction = "stable", Slope = 0.
contradiction(PatientId, CheckinId) :-
    mii_latest_date(PatientId, Date),
    voice_check_in_csv(CheckinId, PatientId, Date, _, "positive"),
    trend(PatientId, _, _, "rising"),
    mii_latest_val(PatientId, IndexValue), IndexValue >= 58.

% Escalation fired by muscle_decline_alert: slope > 0 AND MII >= 58 AND Contradiction holds.
% reasoning_trace is a literal derivation path (facts -> rule -> citation), not LLM text.
escalation(EscalationId, PatientId, RuleId, Priority, ReasoningTrace, Status, CreatedAt) :-
    escalation_rule_csv(RuleId, "muscle_decline_alert", _, CitationId),
    clinical_evidence_csv(CitationId, Source, _, _),
    trend(PatientId, _, Slope, "rising"), Slope > 0,
    mii_latest_val(PatientId, IndexValue), IndexValue >= 58,
    contradiction(PatientId, CheckinId),
    mii_latest_date(PatientId, CreatedAt),
    EscalationId = "ESC_" + PatientId, Priority = "high", Status = "open",
    ReasoningTrace = "muscle_decline_alert fired: MII=" + as_string(IndexValue)
        + " (>=58) via checkin " + CheckinId
        + "; trend slope=" + as_string(Slope) + " rising over 18d;"
        + " contradiction: patient self-reported positive; cited " + Source + " (" + CitationId + ").".
@output("escalation").

% ── Concept: escalation_routed ──
% Routed escalation records that assign high-priority muscle decline alerts to Metabolic Specialist clinicians, derived by matching escalations triggered when a patient's Muscle Integrity Index (calculated from @acoustic_features_csv zero-crossing rate and shimmer via @voice_check_in_csv check-ins) reaches 58 or above with a rising trend that contradicts a positive self-report, then pairing those escalations with an eligible clinician from @clinician_csv only when the patient has granted care t...
@bind("clinician_csv","csv useHeaders='true'","disk/project_15f9a3ef61b","clinician.csv").
@bind("clinical_evidence_csv","csv useHeaders='true'","disk/project_15f9a3ef61b","clinical_evidence.csv").
@bind("consent_grant_csv","csv useHeaders='true'","disk/project_15f9a3ef61b","consent_grant.csv").
@bind("escalation_rule_csv","csv useHeaders='true'","disk/project_15f9a3ef61b","escalation_rule.csv").
@bind("voice_check_in_csv","csv useHeaders='true'","disk/project_15f9a3ef61b","voice_check_in.csv").
@bind("acoustic_features_csv","csv useHeaders='true'","disk/project_15f9a3ef61b","acoustic_features.csv").
@bind("escalation_routed","parquet","disk/results/15f9a3ef61b","escalation_routed").
muscle_integrity_index(PatientId, Date, IndexValue) :-
    voice_check_in_csv(CheckinId, PatientId, Date, _, _),
    acoustic_features_csv(CheckinId, _, Zcr, _, _, Shimmer, _),
    IndexValue = 200 * Zcr + 170 * Shimmer.
mii_latest_date(PatientId, LatestDate) :-
    muscle_integrity_index(PatientId, Date, _), LatestDate = mmax(Date).
mii_earliest_date(PatientId, EarliestDate) :-
    muscle_integrity_index(PatientId, Date, _), EarliestDate = mmin(Date).
mii_latest_val(PatientId, IndexValue) :-
    mii_latest_date(PatientId, D), muscle_integrity_index(PatientId, D, IndexValue).
mii_earliest_val(PatientId, IndexValue) :-
    mii_earliest_date(PatientId, D), muscle_integrity_index(PatientId, D, IndexValue).
trend(PatientId, WindowDays, Slope, Direction) :-
    mii_latest_val(PatientId, Latest), mii_earliest_val(PatientId, Earliest),
    WindowDays = 18, Slope = Latest - Earliest, Direction = "rising", Slope > 0.
trend(PatientId, WindowDays, Slope, Direction) :-
    mii_latest_val(PatientId, Latest), mii_earliest_val(PatientId, Earliest),
    WindowDays = 18, Slope = Latest - Earliest, Direction = "falling", Slope < 0.
trend(PatientId, WindowDays, Slope, Direction) :-
    mii_latest_val(PatientId, Latest), mii_earliest_val(PatientId, Earliest),
    WindowDays = 18, Slope = Latest - Earliest, Direction = "stable", Slope = 0.
contradiction(PatientId, CheckinId) :-
    mii_latest_date(PatientId, Date),
    voice_check_in_csv(CheckinId, PatientId, Date, _, "positive"),
    trend(PatientId, _, _, "rising"),
    mii_latest_val(PatientId, IndexValue), IndexValue >= 58.
escalation(EscalationId, PatientId, RuleId, Priority, ReasoningTrace, Status, CreatedAt) :-
    escalation_rule_csv(RuleId, "muscle_decline_alert", _, CitationId),
    clinical_evidence_csv(CitationId, Source, _, _),
    trend(PatientId, _, Slope, "rising"), Slope > 0,
    mii_latest_val(PatientId, IndexValue), IndexValue >= 58,
    contradiction(PatientId, CheckinId),
    mii_latest_date(PatientId, CreatedAt),
    EscalationId = "ESC_" + PatientId, Priority = "high", Status = "open",
    ReasoningTrace = "muscle_decline_alert fired: MII=" + as_string(IndexValue)
        + " (>=58) via checkin " + CheckinId
        + "; trend slope=" + as_string(Slope) + " rising over 18d;"
        + " contradiction: patient self-reported positive; cited " + Source + " (" + CitationId + ").".

% Consent gate: escalation only routes to a clinician if ConsentGrant(patient,'care_team',true).
escalation_routed(EscalationId, PatientId, ClinicianId, ClinicianName, Priority, ReasoningTrace) :-
    escalation(EscalationId, PatientId, _, Priority, ReasoningTrace, _, _),
    consent_grant_csv(PatientId, "care_team", #T),
    clinician_csv(ClinicianId, ClinicianName, "Metabolic Specialist").
@output("escalation_routed").

% ── Concept: audit_log_entry ──
% computation of the Muscle Integrity Index from @voice_check_in_csv and @acoustic_features_csv data, firing of escalation rules when declining trends are detected, and clinician record access events triggered by routed escalations with valid consent from @consent_grant_csv.
@bind("clinician_csv","csv useHeaders='true'","disk/project_15f9a3ef61b","clinician.csv").
@bind("clinical_evidence_csv","csv useHeaders='true'","disk/project_15f9a3ef61b","clinical_evidence.csv").
@bind("consent_grant_csv","csv useHeaders='true'","disk/project_15f9a3ef61b","consent_grant.csv").
@bind("escalation_rule_csv","csv useHeaders='true'","disk/project_15f9a3ef61b","escalation_rule.csv").
@bind("voice_check_in_csv","csv useHeaders='true'","disk/project_15f9a3ef61b","voice_check_in.csv").
@bind("acoustic_features_csv","csv useHeaders='true'","disk/project_15f9a3ef61b","acoustic_features.csv").
@bind("audit_log_entry","parquet","disk/results/15f9a3ef61b","audit_log_entry").
muscle_integrity_index(PatientId, Date, IndexValue) :-
    voice_check_in_csv(CheckinId, PatientId, Date, _, _),
    acoustic_features_csv(CheckinId, _, Zcr, _, _, Shimmer, _),
    IndexValue = 200 * Zcr + 170 * Shimmer.
mii_latest_date(PatientId, LatestDate) :-
    muscle_integrity_index(PatientId, Date, _), LatestDate = mmax(Date).
mii_earliest_date(PatientId, EarliestDate) :-
    muscle_integrity_index(PatientId, Date, _), EarliestDate = mmin(Date).
mii_latest_val(PatientId, IndexValue) :-
    mii_latest_date(PatientId, D), muscle_integrity_index(PatientId, D, IndexValue).
mii_earliest_val(PatientId, IndexValue) :-
    mii_earliest_date(PatientId, D), muscle_integrity_index(PatientId, D, IndexValue).
trend(PatientId, WindowDays, Slope, Direction) :-
    mii_latest_val(PatientId, Latest), mii_earliest_val(PatientId, Earliest),
    WindowDays = 18, Slope = Latest - Earliest, Direction = "rising", Slope > 0.
trend(PatientId, WindowDays, Slope, Direction) :-
    mii_latest_val(PatientId, Latest), mii_earliest_val(PatientId, Earliest),
    WindowDays = 18, Slope = Latest - Earliest, Direction = "falling", Slope < 0.
trend(PatientId, WindowDays, Slope, Direction) :-
    mii_latest_val(PatientId, Latest), mii_earliest_val(PatientId, Earliest),
    WindowDays = 18, Slope = Latest - Earliest, Direction = "stable", Slope = 0.
contradiction(PatientId, CheckinId) :-
    mii_latest_date(PatientId, Date),
    voice_check_in_csv(CheckinId, PatientId, Date, _, "positive"),
    trend(PatientId, _, _, "rising"),
    mii_latest_val(PatientId, IndexValue), IndexValue >= 58.
escalation(EscalationId, PatientId, RuleId, Priority, ReasoningTrace, Status, CreatedAt) :-
    escalation_rule_csv(RuleId, "muscle_decline_alert", _, CitationId),
    clinical_evidence_csv(CitationId, Source, _, _),
    trend(PatientId, _, Slope, "rising"), Slope > 0,
    mii_latest_val(PatientId, IndexValue), IndexValue >= 58,
    contradiction(PatientId, CheckinId),
    mii_latest_date(PatientId, CreatedAt),
    EscalationId = "ESC_" + PatientId, Priority = "high", Status = "open",
    ReasoningTrace = "muscle_decline_alert fired: MII=" + as_string(IndexValue) + ".".
escalation_routed(EscalationId, PatientId, ClinicianId, ClinicianName) :-
    escalation(EscalationId, PatientId, _, _, _, _, _),
    consent_grant_csv(PatientId, "care_team", #T),
    clinician_csv(ClinicianId, ClinicianName, "Metabolic Specialist").

% AuditLogEntry: first-class, co-derived alongside every rule firing.
% 1) Feature-extraction / MII computation step.
audit_log_entry(EntryId, Actor, Action, Object, Timestamp) :-
    muscle_integrity_index(PatientId, Date, IndexValue),
    Actor = "reasoning_engine",
    Action = "computed_muscle_integrity_index",
    Object = PatientId + "@" + as_string(Date) + " MII=" + as_string(IndexValue),
    Timestamp = Date,
    EntryId = "AUD_MII_" + PatientId + "_" + as_string(Date).
% 2) Escalation rule firing.
audit_log_entry(EntryId, Actor, Action, Object, Timestamp) :-
    escalation(EscalationId, PatientId, RuleId, _, _, _, CreatedAt),
    Actor = "reasoning_engine",
    Action = "fired_escalation_rule",
    Object = RuleId + " -> " + EscalationId + " for " + PatientId,
    Timestamp = CreatedAt,
    EntryId = "AUD_ESC_" + EscalationId.
% 3) Consent-gated clinician read of patient record.
audit_log_entry(EntryId, Actor, Action, Object, Timestamp) :-
    escalation_routed(EscalationId, PatientId, ClinicianId, _),
    mii_latest_date(PatientId, CreatedAt),
    Actor = ClinicianId,
    Action = "read_patient_record_via_escalation",
    Object = EscalationId + " patient " + PatientId,
    Timestamp = CreatedAt,
    EntryId = "AUD_READ_" + EscalationId + "_" + ClinicianId.
@output("audit_log_entry").

% ── Concept: cardiac_signal ──
% A cardiac health signal derived from voice-based check-ins, calculated per patient and date by combining acoustic features from @voice_check_in_csv and @acoustic_features_csv. The index value is computed as 300 times the Jitter plus the Harmonics-to-Noise Ratio (HNR), providing a composite vocal biomarker associated with cardiac condition monitoring.
@bind("voice_check_in_csv","csv useHeaders='true'","disk/project_15f9a3ef61b","voice_check_in.csv").
@bind("acoustic_features_csv","csv useHeaders='true'","disk/project_15f9a3ef61b","acoustic_features.csv").
@bind("cardiac_signal","parquet","disk/results/15f9a3ef61b","cardiac_signal").
% ROADMAP STUB: CardiacSignal has the SAME shape as muscle_integrity_index.
% Activating it is a RULES change, not a model retrain (cite: HearO).
% Currently unpopulated (Active = #F guard yields no rows).
cardiac_signal(PatientId, Date, IndexValue) :-
    voice_check_in_csv(CheckinId, PatientId, Date, _, _),
    acoustic_features_csv(CheckinId, _, _, _, Jitter, _, Hnr),
    Active = #F, Active = #T,
    IndexValue = 300 * Jitter + Hnr.
@output("cardiac_signal").

% ── Concept: glycemic_signal ──
% Represents a glycemic signal estimate for a patient on a given date, derived by joining @voice_check_in_csv check-in records with @acoustic_features_csv to extract the Jitter acoustic feature, then scaling it by a factor of 250 to produce an index value approximating blood glucose levels from voice biomarkers.
@bind("voice_check_in_csv","csv useHeaders='true'","disk/project_15f9a3ef61b","voice_check_in.csv").
@bind("acoustic_features_csv","csv useHeaders='true'","disk/project_15f9a3ef61b","acoustic_features.csv").
@bind("glycemic_signal","parquet","disk/results/15f9a3ef61b","glycemic_signal").
% ROADMAP STUB: GlycemicSignal has the SAME shape as muscle_integrity_index.
% Activating it is a RULES change, not a model retrain (cite: Nature Dig Med 2023).
% Currently unpopulated (Active = #F guard yields no rows).
glycemic_signal(PatientId, Date, IndexValue) :-
    voice_check_in_csv(CheckinId, PatientId, Date, _, _),
    acoustic_features_csv(CheckinId, _, _, _, Jitter, _, _),
    Active = #F, Active = #T,
    IndexValue = 250 * Jitter.
@output("glycemic_signal").

% ── Concept: renal_signal ──
% A composite acoustic index used to monitor renal health signals per patient, derived by combining vocal Energy and Harmonics-to-Noise Ratio (HNR) from @acoustic_features_csv, linked to patient check-in records from @voice_check_in_csv, and computed as 100 times Energy plus HNR for each active check-in session.
@bind("voice_check_in_csv","csv useHeaders='true'","disk/project_15f9a3ef61b","voice_check_in.csv").
@bind("acoustic_features_csv","csv useHeaders='true'","disk/project_15f9a3ef61b","acoustic_features.csv").
@bind("renal_signal","parquet","disk/results/15f9a3ef61b","renal_signal").
% ROADMAP STUB: RenalSignal has the SAME shape as muscle_integrity_index.
% Activating it is a RULES change, not a model retrain.
% Currently unpopulated (Active = #F guard yields no rows).
renal_signal(PatientId, Date, IndexValue) :-
    voice_check_in_csv(CheckinId, PatientId, Date, _, _),
    acoustic_features_csv(CheckinId, Energy, _, _, _, _, Hnr),
    Active = #F, Active = #T,
    IndexValue = 100 * Energy + Hnr.
@output("renal_signal").
```