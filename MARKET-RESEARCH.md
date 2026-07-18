# MedTech Market Research — Validated Pain Points & Defensibility

> Research-backed analysis for a defensible chronic-condition startup / hackathon concept.
> Every claim below is tied to a recent (2023–2025) paper, systematic review, or real-world
> dataset. Companion to [STRATEGY.md](./STRATEGY.md) and [HACKATHON.md](./HACKATHON.md).

---

## The thesis in one paragraph

The money and the medicine now exist to treat obesity, type 2 diabetes, and cardiovascular
disease — but patients **fall out of care between appointments**, and that is where outcomes
collapse. Non-adherence to chronic medication runs at ~50% and costs the US **$100–300B/year**
in avoidable spend. GLP-1 drugs, the biggest thing to happen to metabolic medicine in a
generation, are **abandoned by >50% of patients within a year**. The single strongest
predictor of staying on therapy isn't the drug — it's **contact**: patients with 5+ visits
were **49× more likely** to still be on therapy at 120 days. Human contact is exactly the
thing the system can't scale (obesity-specialist shortage, 3–5 year waitlists). This is the
defensible wedge: **AI that replaces the between-visit human contact that drives adherence** —
now newly possible because of realtime voice agents and multimodal data interpretation that
did not exist two years ago.

---

## 1. The macro pain: non-adherence is a $300B, half-the-patients problem

- WHO: only **~50% of chronic-medication users are adherent**; non-adherence drives worse
  outcomes and higher societal cost. [Frontiers review of systematic reviews, 2025](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC12237677/)
- OECD: non-adherence is linked to **200,000 deaths and €125B avoidable spend/year in Europe**;
  in the US, **$100–300B/year** in avoidable medical expenditure is attributed to adverse drug
  events, ~⅓ of it to non-adherence. [Frontiers, 2025](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC12237677/)
- Per-person annual cost of disease-specific non-adherence ranges **$949–$44,190**;
  all-cause **$5,271–$52,341**. [Economic impact systematic review, PMC](https://pmc.ncbi.nlm.nih.gov/articles/PMC5780689/)

**Why it matters:** adherence isn't a soft "engagement" metric — it is the direct lever on
mortality and cost. A tool that moves it has a hard ROI story for payers and providers.

---

## 2. The sharpest wedge: the GLP-1 "messy middle"

GLP-1s (semaglutide/Wegovy, tirzepatide/Zepbound) are a **$100B market** with **$2.8B** raised
by venture-backed obesity/GLP-1 companies — but the clinical reality is a leaky bucket.

- **>50% discontinue within 1 year** (125,000-patient US EHR study; Danish national cohort
  found 52% for non-diabetes weight-loss use). Only **14% remain on Wegovy at 3 years**.
  [Medscape / real-world study](https://www.medscape.com/viewarticle/real-world-study-finds-over-50-stop-glp-1s-within-1-year-2025a1000obm) · [medRxiv 125k cohort](https://www.medrxiv.org/content/10.1101/2024.07.26.24311058.full.pdf)
- **Side effects are the #1 reason (28.2%)** for stopping. GI effects are near-universal:
  nausea 25–44%, diarrhea 19–30%, vomiting 8–24%, constipation 17–24%.
  [Optimizing GLP-1 therapies, PMC 2025](https://pmc.ncbi.nlm.nih.gov/articles/PMC12661421/)
- **Muscle loss:** as much as **40–60% of weight lost on GLP-1s is lean muscle mass** — a
  long-term cardiometabolic and functional risk the drug does nothing to counter.
  [Sword Health](https://swordhealth.com/articles/glp-1-side-effect-management-muscle-loss) · [Mayo Clinic](https://store.mayoclinic.com/education/glp-1-medications-and-muscle-loss-what-to-know-about-nutrition-and-supplements/)
- **Nutrition support is missing:** a **joint advisory from 4 major bodies (ACLM, ASN, OMA,
  TOS)** says protein-forward, fiber-rich intake + resistance training + side-effect
  management are essential — yet **payer coverage for medical nutrition therapy is limited**,
  so most patients get the drug and no support. [AJCN joint advisory, 2025](https://ajcn.nutrition.org/article/S0002-9165(25)00240-0/fulltext)
- **The killer stat for defensibility:** patients with **5+ follow-up visits were 49× more
  likely to remain on therapy at 120 days.** [AJMC — Gaps in persistence](https://www.ajmc.com/view/gaps-in-persistence-coverage-limit-glp-1-impact-in-obesity)

**The gap:** the drug is the easy part. The hard part — managing side effects day to day,
protecting muscle, staying motivated through week 6's nausea — is what determines whether the
patient is one of the 50% who quit. Right now that support only exists via human clinicians.

---

## 3. The supply side can't scale the one thing that works (contact)

- **Obesity-specialist shortage:** only **9,895 board-certified obesity medicine physicians**
  in the US (Dec 2024), and they're **disproportionately absent from the highest-obesity
  counties.** [Mayo Clinic Proceedings, 2025](https://www.mayoclinicproceedings.org/article/S0025-6196(25)00574-9/fulltext)
- **Doctors are undertrained:** >40% of Americans have obesity, but clinicians historically
  got little obesity-care training; "if an untrained clinician prescribes without proper
  support, somebody could get sicker." [Axios, 2024](https://www.axios.com/2024/05/28/us-doctors-obesity-health-care-training)
- **Waitlists of 3–5 years** for specialist obesity services in parts of the UK.
  [Rapid review, medRxiv 2024](https://www.medrxiv.org/content/10.1101/2024.11.07.24316892.full.pdf)

**Why it matters:** the intervention that works (frequent contact) is exactly the one the
system is structurally unable to deliver. That is the space AI can fill — not by replacing the
clinician's judgment, but by delivering the *frequency* humans can't.

---

## 4. Adjacent validated gaps (alternative or complementary wedges)

**a) Home biomarker data is collected but not interpreted.**
- RPM for hypertension is growing fast (**~30M US patients / 11.2% by 2024**) and improves
  BP control, but clinic/self-report readings are "too intermittent," and **self-measurement
  is itself a burden**, especially for older adults. [RPM cohort, PMC](https://pmc.ncbi.nlm.nih.gov/articles/PMC11353537/) · [Challenges & barriers, CJNI](https://cjni.net/journal/?p=13479)
- Patients **can't interpret numeric results**: a 2024 JMIR systematic review found abnormal
  or unexplained results commonly cause **anxiety, confusion, and unnecessary consultations**;
  personalized reference ranges reduce both confusion and needless physician contact.
  [JMIR 2024](https://www.jmir.org/2024/1/e53993)
- Emerging science: **LLMs can turn CGM time-series into plain-language, actionable advice**
  (LLM-CGM benchmark; personalized recommendation frameworks, 2025). The tech to close the
  interpretation gap has just arrived. [LLM-CGM benchmark](https://www.worldscientific.com/doi/abs/10.1142/9789819807024_0007) · [Personalized CGM+LLM, arXiv](https://arxiv.org/pdf/2510.04386)

**b) Women's cardiovascular care is systematically under-served.**
- Women are **~2× more likely to be misdiagnosed after a heart attack** and **30% more
  likely to have stroke symptoms missed in the ED**; less likely to get imaging, PCI, or
  statins at equal risk. [JACC State-of-the-Art, 2024](https://www.jacc.org/doi/10.1016/j.jacc.2024.04.028) · [Ischemic heart disease gender disparities, PMC](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC12425171/)
- A tool that helps women recognize non-classic symptoms and escalate appropriately targets
  a large, evidence-backed, under-addressed gap.

---

## 5. Why now — the enabling tech that didn't exist two years ago

Innovation judges (and investors) reward "couldn't have been built in 2022." Three capabilities
crossed the line recently:

- **Realtime speech-to-speech voice agents.** A real-world deployment (RadiantGraph × Oshi
  Health, ~8,800 members, Q4 2025) saved **~75 staff-hours per 1,000 contacts**; another
  study showed AI voice agents **lifted specialty-care enrollment 340%**; 70% of patients
  accepted AI monitoring, 37% preferred it to traditional. [RadiantGraph/Oshi PRNewswire](https://www.prnewswire.com/news-releases/study-finds-ai-voice-agents-increased-specialty-care-program-enrollment-rates-340-in-real-world-clinical-setting-302818900.html) · [npj Digital Medicine, 2025](https://www.nature.com/articles/s41746-025-01776-y)
- **Multimodal interpretation** (vision + LLM) turns photos of meals/devices/labs into
  structured, explained data — attacking both the logging-friction and interpretation gaps.
- **AI medication reminders already show a 20–30% adherence lift** in the literature — a
  conservative floor for what conversational agents can do. [Parloa healthcare review](https://www.parloa.com/blog/ai-voice-agents-in-healthcare/)

And the counter-evidence to respect: **only ~2% of a large real-world app cohort had
sustained continuous use**; digital-health dropout runs **19–45%**. [Attrition meta-analysis, PMC](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC7556375/) · [Scoping review, PMC 2025](https://pmc.ncbi.nlm.nih.gov/articles/PMC12803440/) — Any solution has to be *proactive* (it reaches out) rather than another app the patient must remember to open.

---

## 6. Where defensibility actually comes from (medtech moats)

A hackathon idea only becomes a startup if it has a moat. For this space, the real moats are:

1. **Proprietary longitudinal data / outcomes.** Twin Health raised **$53M** on a clinically
   validated "AI digital twin" — the moat is the validated outcome dataset, not the app.
   [Twin Health, PRNewswire](https://www.prnewswire.com/news-releases/twin-health-announces-53m-investment-as-ai-digital-twin-unlocks-sustainable-diabetes-and-obesity-outcomes-while-reducing-medications-302535848.html) The more a system runs, the better its personalization and the harder to copy.
2. **The full-programme (drug + support) model.** Investors explicitly note that GLP-1 +
   diet + activity + behavioral support keeps patients adherent — the wraparound, not the
   prescription, is the durable business. [FierceHealthcare](https://www.fiercehealthcare.com/health-tech/telehealth-companies-target-100b-weight-loss-drug-market-patients-grapple-access-costs)
3. **Clinical validation & regulatory posture.** A tool that *triages to* clinicians (not
   diagnoses) is deployable now and de-risked on liability — the fastest path to real-world use.
4. **Payer ROI story.** Because non-adherence has a hard dollar cost, an adherence lift is
   directly sellable to payers/employers — a clearer business model than consumer wellness.

---

## 7. Recommended wedge (most-validated, most-defensible)

**"The between-visit layer for GLP-1 / metabolic programmes."** A proactive AI companion that
delivers the *frequency of contact* proven to drive adherence, focused on the exact failure
points the research identifies:

- **Manages side effects in the moment** (the #1 reason 28% quit) — proactive check-ins on
  nausea/GI symptoms with evidence-based guidance and escalation when red-flagged.
- **Protects muscle & nutrition** (the 40–60% lean-mass problem) — protein/fiber nudges +
  resistance-training prompts, the support the joint advisory says is missing.
- **Interprets home data** (BP, weight, CGM, labs) into plain, personalized, actionable
  language — closing the interpretation gap.
- **Bridges to the clinician** with a drafted summary only when patterns warrant — solving
  the specialist-shortage/frequency problem without adding clinician load.

**The research chain that backs it (the pitch's spine):**
> 50% quit within a year → side effects are the #1 cause → the one thing that prevents quitting
> is frequent contact (49× at 120 days) → but specialists are too scarce to deliver it →
> realtime voice AI can now deliver human-feeling contact at scale (75 staff-hrs saved/1,000
> contacts; 340% enrollment lift) → therefore: an AI between-visit layer is both needed and
> newly possible.

Every link in that chain is a citation above. That is what "research-backed defensibility"
looks like.

---

## 8. Risks & honest counter-arguments

- **Engagement decay is real** (2% sustained use). Mitigation: proactive outreach, not a
  passive app; tie contact to medication timing and symptom moments.
- **Incumbents are moving** (Twin, Noom, virtual GLP-1 clinics). Differentiation: own the
  *side-effect + muscle + interpretation* micro-moment that generic coaching apps skip, and
  the clinical-escalation bridge.
- **Clinical safety / liability.** Never diagnose; always triage to a human. Position as an
  adherence & monitoring layer, FDA-light.
- **Evidence bar.** The category now expects real outcome data — plan an early validation
  study; it's the moat.

---

## Source index (primary)
- Non-adherence economics: [Frontiers 2025](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC12237677/) · [Systematic review PMC](https://pmc.ncbi.nlm.nih.gov/articles/PMC5780689/)
- GLP-1 discontinuation: [Medscape](https://www.medscape.com/viewarticle/real-world-study-finds-over-50-stop-glp-1s-within-1-year-2025a1000obm) · [medRxiv 125k](https://www.medrxiv.org/content/10.1101/2024.07.26.24311058.full.pdf) · [Persistence gaps / 49× AJMC](https://www.ajmc.com/view/gaps-in-persistence-coverage-limit-glp-1-impact-in-obesity)
- GLP-1 side effects / muscle / nutrition: [Optimizing GLP-1, PMC](https://pmc.ncbi.nlm.nih.gov/articles/PMC12661421/) · [AJCN joint advisory](https://ajcn.nutrition.org/article/S0002-9165(25)00240-0/fulltext) · [Sword Health](https://swordhealth.com/articles/glp-1-side-effect-management-muscle-loss)
- Clinician shortage: [Mayo Clinic Proceedings](https://www.mayoclinicproceedings.org/article/S0025-6196(25)00574-9/fulltext) · [Axios](https://www.axios.com/2024/05/28/us-doctors-obesity-health-care-training)
- RPM / data interpretation: [RPM cohort PMC](https://pmc.ncbi.nlm.nih.gov/articles/PMC11353537/) · [JMIR lab results 2024](https://www.jmir.org/2024/1/e53993) · [LLM-CGM benchmark](https://www.worldscientific.com/doi/abs/10.1142/9789819807024_0007)
- Women's CVD: [JACC 2024](https://www.jacc.org/doi/10.1016/j.jacc.2024.04.028) · [Gender disparities PMC](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC12425171/)
- Voice AI validation: [RadiantGraph/Oshi](https://www.prnewswire.com/news-releases/study-finds-ai-voice-agents-increased-specialty-care-program-enrollment-rates-340-in-real-world-clinical-setting-302818900.html) · [npj Digital Medicine](https://www.nature.com/articles/s41746-025-01776-y)
- Engagement decay: [Attrition meta-analysis](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC7556375/) · [Scoping review 2025](https://pmc.ncbi.nlm.nih.gov/articles/PMC12803440/)
- Market/defensibility: [Twin Health $53M](https://www.prnewswire.com/news-releases/twin-health-announces-53m-investment-as-ai-digital-twin-unlocks-sustainable-diabetes-and-obesity-outcomes-while-reducing-medications-302535848.html) · [FierceHealthcare metabolic](https://www.fiercehealthcare.com/health-tech/telehealth-companies-target-100b-weight-loss-drug-market-patients-grapple-access-costs)
