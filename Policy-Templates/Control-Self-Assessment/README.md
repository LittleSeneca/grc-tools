# Control Self Assessment Process

## What This Is

The Control Self Assessment (CSA) process defines how control owners evaluate their own controls on a regular cadence. Rather than waiting for an annual audit to discover control failures, CSAs catch issues early and build a culture of control ownership. This is a core component of SOC 2 continuous monitoring.

## What It Covers

- Control inventory maintenance
- Assessment scheduling and cadence
- Assessment execution criteria (design effectiveness, operating effectiveness, evidence)
- Rating system (Effective / Needs Improvement / Ineffective)
- Remediation tracking for deficient controls
- Reporting to the oversight body
- CSA worksheet template

## Gotchas People Get Wrong

**1. Control owners rating their own controls as "Effective" by default.** This is the biggest problem with CSAs. People don't want to flag their own controls as deficient. Mitigate this with: (a) random quality reviews by a second party, (b) requiring evidence submission with every assessment, and (c) making it clear that finding and fixing issues is good — hiding them is bad.

**2. Evidence is the hardest part.** Control owners often know their control is working but can't produce evidence. "I check it every week" isn't evidence unless there's a log or record. Build evidence collection into the control, not the assessment.

**3. Quarterly CSA cadence sounds good but burns people out.** If you have 50+ controls and assess them all quarterly, control owners spend more time assessing controls than operating them. Consider a risk-based approach: high-risk controls quarterly, medium semi-annually, low annually.

**4. No one reads the CSA report.** If the Oversight Body doesn't review CSA results, the program is theater. Make sure the summary report goes to a body that actually meets and has authority.

**5. "Needs Improvement" becomes a permanent state.** Some controls sit in "Needs Improvement" for years because there's no forcing function. Set a maximum time (e.g., 90 days) for remediation before auto-escalation.

## Implementation Advice

- **Start with a small set of controls.** Don't try to CSA your entire control set in month one. Pilot with 5-10 critical controls, refine the process, then expand.
- **Use a tool.** Spreadsheets work for 10 controls. They don't work for 50+. A GRC platform or compliance automation tool (Drata, Vanta, Secureframe) makes CSAs dramatically easier.
- **Tie CSAs to annual policy review.** When you review a policy, CSA the controls that enforce it. This creates a natural review rhythm.
- **Celebrate honest assessments.** If a control owner flags their own control as "Ineffective" and fixes it, that's a win. If they hide it and the auditor finds it, that's a problem. Make this cultural norm explicit.
