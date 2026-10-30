### 1. Which parts should use AI versus deterministic rules?

The system uses conventional PDF extraction to retrieve text and tables, with OCR for scanned documents where necessary. AI then interprets the extracted content according to the administrator's configured criteria, identifies relevant evidence, and returns structured information with source references and uncertainty flags.

The administrator's criteria, weights, and thresholds are retrieved directly through application code rather than AI. For mandatory requirements, AI identifies evidence of compliance, non-compliance, or missing information, while deterministic rules classify proposals as eligible, ineligible, or requiring human review.

For weighted evaluation, AI extracts qualifying experience, pricing, timelines, warranty details, and evidence for the team-and-approach rubric. The rules engine applies the configured scoring formulas and calculates final scores.

This separation uses AI where language interpretation is necessary and deterministic code where consistency, accuracy, and repeatability are essential.

### 2. How would you ensure repeatability and explainability?

The system uses fixed, versioned instructions, evaluation criteria, and scoring formulas to maintain consistency. AI extracts information in a structured format with supporting evidence and page references. Its outputs are validated, and uncertain or contradictory information is flagged for human review.

Since AI extraction can vary between runs, the system preserves verified evidence and uses deterministic code to calculate scores. Each batch stores its configuration version, extracted values, scoring breakdown, and review decisions.

For explainability, reviewers can inspect each criterion's score, the formula used, the supporting proposal evidence, and the reason for any eligibility decision or flag. This allows results to be audited and reproduced without depending on a fresh AI interpretation.

### 3. How would you handle human oversight and auditing?

The system is designed to assist procurement reviewers, not replace them. It reduces manual work by extracting proposal information, checking requirements, calculating weighted scores, and recommending the best-fitting contractor based on the configured criteria.

However, the highest-scoring proposal is not automatically awarded the contract. The final decision remains with the human reviewer, who can select a different eligible contractor with a documented justification.

The system maintains an audit trail containing the evaluation criteria, extracted evidence, scores, flagged issues, reviewer corrections, and final decisions. If evidence or scoring is incorrect, the reviewer can request a correction and recalculation without silently manipulating scores. This ensures accountability, transparency, and meaningful human oversight.

### 4. How would the system handle changing evaluation criteria?

Evaluation criteria are stored as versioned configurations, and each batch maintains a history of the criteria and results used during evaluation.

When an administrator updates the criteria, they can choose whether the changes apply to future batches or an active batch, subject to procurement policy. If applied to an active batch, the system invalidates the previous provisional results and re-evaluates every proposal using the same updated configuration.

Changes to scoring weights require recalculation, while changes to eligibility requirements or evidence definitions may require additional extraction and validation. Previous configurations, scores, and decisions remain stored for auditing. The system then produces a new provisional ranking until all outstanding issues are resolved.

### 5. What are the main risks and limitations?

AI-assisted evaluation can reduce manual work, but it cannot completely eliminate errors. AI may misinterpret proposal evidence, overlook information, or be influenced by malicious instructions embedded in PDFs. These risks can be reduced through structured extraction, source references, validation, and human review.

Even when extraction and calculations are correct, rankings may still be misleading if the configured criteria or weights do not accurately reflect procurement priorities. Numerical scores also cannot capture every aspect of contractor suitability.

Additional safeguards improve reliability but increase system complexity. Therefore, the design prioritizes transparent scoring, configurable criteria, clear uncertainty flags, audit records, and human oversight rather than attempting to automate every decision.