# 🏥 Hospital Admission Readiness Simulator

A single-file, browser-based healthcare training simulation for Hospital Admission Coordinators. Built with HTML, Tailwind CSS CDN, and Vanilla JavaScript — zero dependencies, zero backend.

> **Training use only.** All provider names, payer names, and clinical figures are illustrative. No real patient data is collected or stored.

---

## Overview

AdmissionSim Pro puts the user in the role of a Hospital Admission Coordinator, walking through a realistic end-to-end admission workflow — from initial setup through prior authorization, clinical documentation, bed assignment, care coordination, and final admit decision. A live readiness score tracks progress and enforces real-world constraints (CMS rules, InterQual/Milliman criteria, PA denial caps).

---

## Screenshot

<!-- Replace the URL below with your actual screenshot path once uploaded to your repo -->

![Admission Readiness Simulator – Setup Phase](https://github.com/srivastavarudresh552-cmyk/ClaudeAIChallenge/blob/main/Day28/Screenshot%202026-06-29%20131315.png)
![Admission Readiness Simulator – Setup Phase](https://github.com/srivastavarudresh552-cmyk/ClaudeAIChallenge/blob/main/Day28/Screenshot%202026-06-29%20131331.png)
![Admission Readiness Simulator – Setup Phase](https://github.com/srivastavarudresh552-cmyk/ClaudeAIChallenge/blob/main/Day28/Screenshot%202026-06-29%20131408.png)
![Admission Readiness Simulator – Setup Phase](https://github.com/srivastavarudresh552-cmyk/ClaudeAIChallenge/blob/main/Day28/Screenshot%202026-06-29%20131430.png)
![Admission Readiness Simulator – Setup Phase](https://github.com/srivastavarudresh552-cmyk/ClaudeAIChallenge/blob/main/Day28/Screenshot%202026-06-29%20131457.png)
![Admission Readiness Simulator – Setup Phase](https://github.com/srivastavarudresh552-cmyk/ClaudeAIChallenge/blob/main/Day28/Screenshot%202026-06-29%20131508.png)
---

## Features

### Setup Phase
Collects seven fields before analysis begins:
- **Provider / Facility Name** — illustrative training label
- **Attending Physician** — illustrative training label
- **Primary Diagnosis** — Acute MI, CHF, Pneumonia, Elective Surgery, Hip Fracture
- **Admission Type** — Inpatient, Observation, Emergency, ICU, Same-Day Surgery
- **PA (Prior Authorization) Status** — Approved, Pending, Denied
- **Insurance / Payer** — illustrative training label
- **Admission Date** — defaults to today

### Regulatory Notices
Two context-sensitive alerts fire automatically based on selections:

**Observation Status** triggers the CMS 2-Midnight Rule notice:
> *"CMS 2-Midnight Rule applies — different cost-sharing, SNF eligibility, and billing than inpatient. Medicare patients require written MOON notification."*

**Acute MI / CHF** triggers the InterQual/Milliman criteria alert:
> *"InterQual/Milliman thresholds apply — ensure documentation meets medical necessity standards before UR review."*

---

## Scoring System

### Readiness Score Weighting

| Component | Weight |
|---|---|
| PA Status | 25% |
| Clinical Documentation | 20% |
| Physician Orders | 20% |
| Insurance Verification | 15% |
| Consent Forms | 10% |
| Bed Assignment | 10% |

### Score Behavior
- **Initial analysis** clamps to **30–60%** — the final decision is not revealed on first render.
- **Denied PA + ICU** admission is hard-capped at **68%** — administrative actions alone cannot clear 70%. Clinical reclassification is required.
- **Denied PA (non-ICU)** without a successful appeal is capped at **72%**.
- Score updates live as each workflow action and PA step is completed.

---

## PA (Prior Authorization) Branches

### Approved
Clear path — proceed with workflow actions.

### Pending
Three required steps to progress:
1. Follow Up with Payer
2. Upload Supporting Documentation
3. Contact Attending Physician

Each step incrementally raises the PA contribution to the score (capped at 18% while pending).

### Denied
Three sequential steps to unlock the appeal:
1. Review Denial Reason *(must be completed first)*
2. Contact Insurance *(must be completed second)*
3. Submit Appeal *(unlocks only after steps 1 and 2)*

A successful appeal converts PA status to Approved, removes the score cap, and re-renders the PA branch.

---

## Workflow Actions

Seven actions are available after initial analysis. Each completes exactly once and advances the admission timeline:

| Action | Score Impact |
|---|---|
| 🛏 Assign Bed | Bed risk drops to Low |
| 🔍 Verify Insurance | Insurance score fully credited |
| 📁 Upload Documentation | Documentation score fully credited |
| ✍ Complete Consent | Consent score fully credited |
| 👨‍⚕️ Contact Physician | Physician orders credited, documentation boosted |
| 💉 Notify Nursing | Orders score incremented |
| 🚑 Prepare Patient Arrival | Bed score incremented |

---

## Admission Timeline

Nine milestones displayed as a step-indicator, advancing one step per completed workflow action:

PA Review → Insurance Verify → Bed Assign → Documentation → Consent → Patient Arrival → Registration → Clinical Assessment → Admission Complete

---

## Care Coordination Cards

Five team members displayed with role-specific context:

- **Attending Physician** — orders, documentation, clinical decisions
- **Case Manager** — care transitions, LOS management, post-acute planning
- **Nursing — Floor Lead** — unit preparation, bed assignment notification
- **Utilization Review** — concurrent review, denial risk identification, InterQual and Milliman criteria application
- **Discharge Planner** — SNF eligibility, home health, DME needs assessment

---

## Risk Tracking

Four risk indicators update dynamically as actions are completed:

| Risk | Elevated When |
|---|---|
| Documentation Risk | Upload Documentation not completed |
| Insurance Risk | Verify Insurance not completed |
| Bed Risk | Assign Bed not completed |
| Clinical Risk ★ | Diagnosis is Acute MI / CHF, or Admission Type is ICU — weighted higher |

---

## Governance Snapshot

Unlocks automatically when readiness reaches **≥ 75%**:

| Benchmark | Value |
|---|---|
| PA Turnaround | 3–5 days (industry estimate) |
| Inpatient Denial Rate | ~8–10% (CMS estimate) |
| PA Rework Cost | ~$11 / transaction (CAQH Index) |

> Estimates only — benchmarks vary. For training purposes only.

---

## Final Decision

Renders when five or more workflow actions are completed, or readiness reaches ≥ 75%.

| Score | Outcome |
|---|---|
| **≥ 90%** | ✅ **Admit** — full admission summary with all field values |
| **< 90%** | ⚠ **Not Ready** — lists missing actions, unresolved PA items, remaining risks |

---

## Tech Stack

| Layer | Choice |
|---|---|
| Markup | Semantic HTML5 |
| Styling | Tailwind CSS CDN + custom CSS variables |
| Logic | Vanilla JavaScript (no frameworks) |
| Fonts | IBM Plex Sans, IBM Plex Mono, IBM Plex Serif (Google Fonts) |
| Deployment | Any static host — single `.html` file, no build step |

---

## Deployment

Because the app is a single self-contained HTML file:

```bash
# Serve locally
npx serve .

# Or open directly in a browser
open admission-readiness-simulator.html
```

Compatible with Vercel, Netlify, GitHub Pages, or any static host — drop the file and it works.

---

## File Structure

```
admission-readiness-simulator.html   # Complete app — HTML + CSS + JS in one file
README.md                            # This file
```

---

## Compliance & Training Notes

- All payer and provider names are explicitly labeled as **illustrative training data**.
- The CMS 2-Midnight Rule, MOON notification requirement, InterQual, and Milliman references are educational prompts — not legal or clinical advice.
- The governance benchmarks (CMS denial rate, CAQH rework cost) are cited estimates for training context only.
- No patient data, PHI, or real authorization data is entered or stored.

---

## License

For internal training and educational use. Not intended for production clinical decision-making.
