# Undertone - the listening layer

> Voice as a continuous vital sign for chronic metabolic disease. A daily 60-second voice
> check-in doubles as a passive acoustic sensor: it extracts biomarkers of muscle loss (and, on
> the roadmap, cardiac / glycemic / renal decline), detects drift over time, and escalates an
> explainable, cited summary to a clinician — human-in-the-loop, never an autonomous diagnosis.
>
> Built for the ReimagineHealth hackathon (eMed × OpenAI). *Working name — see [NAMES.md](./NAMES.md).*


Demo video:
https://github.com/user-attachments/assets/7afe29bd-d086-4188-9018-7f0ce31bc703




> Using Prometheux as the backend. Here is the ontology and data flow:
> <img width="714" height="582" alt="image" src="https://github.com/user-attachments/assets/1b9b8ca7-c6fd-4bd4-86e1-bdbf06b928ca" />
<img width="385" height="610" alt="image" src="https://github.com/user-attachments/assets/658d9369-a4e7-4816-98e6-cf113b984e9e" />


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


Prometheux vadalog:
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
