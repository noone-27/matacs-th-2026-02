<div align="center">

AI-Assisted Contractor Proposal Triage
A configurable, evidence-driven decision-support design for contractor proposal evaluation

MATACS Technical Assessment · MATACS-TH-2026-02
</div>

Scope: This repository contains a system design and illustrative evaluation, not a deployed application. AI assists with extracting and interpreting proposal evidence; deterministic code applies the scoring policy; a human remains responsible for review and award decisions.

Assessment details
Field	Detail
Candidate	Danish Ahmed
Assessment reference	MATACS-TH-2026-02
Time actually spent	[Enter actual time spent before submitting]
Deliverable	Architecture, design rationale, extraction prompts, and illustrative ranking
Example scenario	150 kW warehouse solar installation in Windsor, Ontario


The problem
Evaluating contractor proposals requires reviewers to compare differently formatted PDFs against the same requirements, explain why each contractor received its score, and identify incomplete or suspicious information without surrendering procurement decisions to an AI model.

The proposed system lets an administrator configure category-specific requirements and scoring weights. A reviewer uploads proposal PDFs, receives a provisional evidence-backed ranking, resolves review flags, and records the final human decision.
Architecture

Evaluation flow
1. Configure — An administrator defines mandatory requirements, weights, rubrics, and versioned evaluation settings.
2. Ingest — A reviewer uploads PDFs and selects the proposal category. Standard PDF parsing extracts text, tables, and page references; OCR handles scans when needed.
3. Extract evidence — AI returns structured values, qualitative rubric classifications, source references, uncertainties, and suspected instruction-injection flags. Proposal text is treated as untrusted data.
4. Validate and score — Application code checks mandatory requirements, applies the configured formulas, and calculates an auditable provisional ranking.
5. Review and decide — A human resolves missing or disputed evidence and security flags. Authorized corrections trigger recalculation; the final award decision is recorded separately.
Evaluation policy used in this example
The assessment provides the criterion weights; the detailed formulas below are illustrative design choices, not additional rules claimed to come from the assessment brief.
Criterion	Weight	Illustrative scoring policy
Relevant experience	30%	30 × qualifying installations / highest eligible qualifying count
Evaluated price	25%	25 × lowest eligible evaluated price / proposal evaluated price
Delivery timeline	15%	15 points at ≤10 weeks; linearly decreases to 10 points at 16 weeks; missing timeline gets provisional zero plus review flag
Warranty	15%	7.5 points each for panel and workmanship duration, relative to eligible maxima
Team and approach	15%	Five components × 3 points: lead, team roles, safety, methodology, monitoring/reporting


Mandatory eligibility checks: the bidder itself states that it holds the required Ontario electrical contractor licence, WSIB clearance, and CAD $5 million liability insurance. The system evaluates what is stated in the proposal, not whether the claims have been independently verified. External credential verification remains a separate human-controlled process.
Team-and-approach rubric
Each component is classified as absent (0), basic (1), specific (2), or comprehensive (3). AI extracts and classifies supporting evidence; application code converts the classifications into points. Equal component weights are used because no more granular allocation was supplied.

The following are illustrative provisional classifications from the supplied proposal summaries, not verified output from a live AI run:
Component (maximum 3 each)	A	C	D	E
Named lead	2	2	2	2
Team roles	1	1	0	1
Safety	2	0	2	2
Methodology	0	0	2	2
Monitoring / reporting	2	2	0	2
Total / 15	7	5	6	9


Interpretation and limitations: A named lead is treated as specific (2), but the summaries do not establish comprehensive responsibility details. Crew headcount alone supports only basic team information (1), not clearly assigned roles. Safety mentions in A, D, and E are provisionally treated as specific (2), although the summaries do not include full safety procedures; these classifications require evidence verification. D's structural review and E's roof-load assessment are treated as specific methodology elements (2), not complete implementation plans. A's 12-month monitoring, C's weekly reporting, and E's 24-month monitoring each support a specific monitoring/reporting classification (2). These judgments are contestable and must be checked against actual cited PDF passages before a final evaluation.
Illustrative provisional ranking
Rank	Proposal	Experience /30	Price /25	Timeline /15	Warranty /15	Team /15	Total /100	Review status
1	A	30.00	24.26	13.33	13.75	7	88.35	Ranked
2	E	17.14	23.81	12.50	15.00	9	77.46	Ranked
3	C	19.29	23.67	11.67	13.75	5	73.37	Ranked; security review required
4	D	23.57	25.00	0.00	13.75	6	68.32	Flagged; timeline missing
—	B	—	—	—	—	—	Not scored	Ineligible


Calculations use unrounded intermediate values; totals are rounded to two decimals. Only mandatory-eligible proposals participate in the relative experience, price, and warranty baselines. D remains in that pool because its timeline omission is a scored review issue rather than a mandatory eligibility failure.

Why the exceptions matter:
- B: The bidder does not state that it holds the required electrical contractor licence itself; a future subcontractor's licence does not meet that mandatory condition.
- C: An instruction embedded in the proposal attempts to manipulate the AI's ranking. The system ignores that instruction, evaluates legitimate evidence, and records a separate security-review flag.
- D: No completion timeline is specified. Under the chosen provisional policy, D receives 0/15 for timeline, stays in the numerical ranking, and must be reviewed before finalization. An authorized evidence correction triggers recalculation rather than a manual score edit.
The ranking is decision support, not an award decision. The review flags and rubric assumptions must be resolved or accepted under the applicable procurement policy before finalization.
Reliability, security, and auditability
- Untrusted inputs: PDF contents are evidence to inspect, never instructions to execute.
- Traceable evidence: Store source page references, extracted values, uncertainty, and scoring explanations.
- Deterministic scoring: Code, not the AI model, performs arithmetic and mandatory-rule enforcement.
- Reproducibility: Version criteria, prompts, model details, source hashes, and scoring outputs; reuse verified evidence for repeatable recalculation.
- Human oversight: Flag missing, conflicting, or suspicious information; log reviewer actions and justifications.
- Change management: Apply new configuration versions to future batches by default; if an active batch is re-evaluated, rescore the entire batch consistently and preserve history.
Repository guide
File	Purpose
[`WRITEUP.md`](WRITEUP.md)	Answers to the five design questions, scoring assumptions, and safeguards
[`PROMPTS.md`](PROMPTS.md)	Six criterion-specific AI evidence-extraction prompts
[`ranking.json`](ranking.json)	Machine-readable provisional ranking for proposals A–E
[`diagram/flowchart.png`](diagram/flowchart.png)	System architecture diagram


AI-use disclosure
I used ChatGPT to challenge my architecture decisions, compare scoring alternatives, and help draft the explanation, extraction prompts, diagram, and illustrative calculations. I disagreed with an early timeline-scoring proposal that awarded only 3/15 to a contractor meeting the 16-week limit, and selected a 10/15 baseline instead. The resulting design keeps eligibility and scoring in deterministic code and requires human verification of uncertain evidence and qualitative classifications.
Submission note: Replace the time-spent placeholder with your actual time, verify the disclosure reflects your own work, and review the files before publishing. This repository intentionally does not reproduce the assessment brief or complete fictional proposal texts.`