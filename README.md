FAIR-AI | Evidence-Based AI Assurance & Risk Analytics
Independent applied research by Uju Judith Eziokwu, Founder of Voidborn Tech
Research context: Financial services AI governance · Citi public-information case study
Status: Exploratory, synthetic, reproducible research — not a validated compliance product
Central question: How can organisations distinguish AI systems that are genuinely low-risk from those that only appear safe because the evidence needed to assess them is incomplete?

Overview
An AI risk dashboard may report a falling error rate even as underlying errors increase. One reason is selective observation: decisions made by a system can influence which cases are reviewed, which outcomes become visible, and therefore which cases appear in its performance metrics.
FAIR-AI investigates this measurement problem through six Python-based simulation experiments using illustrative banking workflows. The research examines assurance controls, evidence continuity across agent-like workflow stages, monitoring bias, missing potential outcomes, audit sampling and uncertainty-aware deployment decisions.
The aim is to explore a practical governance question: before approving or scaling an AI system, what evidence is available, what remains unobserved, and which conclusions can the evidence actually support?
This repository contains executable research code and synthetic outputs. It is not an audit of Citi, a deployed multi-agent banking system, or proof of regulatory compliance.
Why Citi?
Citi was selected as a public-information case study because the scale and complexity of global banking offer a useful context for discussing AI governance, model risk management, operational risk and accountability. The case study connects the general research questions to challenges that can arise in complex financial institutions; it does not evaluate Citi's actual AI systems, policies or controls.
Independence statement: This work was developed independently through Voidborn Tech. It is not commissioned, sponsored, endorsed or validated by Citi. No Citi internal systems, customer data, proprietary code, models or confidential documentation were accessed or tested.
Research questions
1. Can synthetic assurance controls detect predefined control failures, and what does that demonstrate—or fail to demonstrate—about real-world effectiveness?
2. Can evidence gaps between connected workflow stages weaken an apparently complete assurance record?
3. How does selective observation distort measured AI performance?
4. Can observed error improve while the simulator-known underlying error deteriorates?
5. What can be estimated when outcomes for rejected or unselected cases are not observable?
6. How should deployment decisions balance risk limits, evidence uncertainty and the cost of excessive caution?
Experiments at a glance
Experiment	Research focus	Principal interpretation
01 — Assurance controls	Five illustrative control tests in a synthetic banking workflow	Tests rule-based detection under constructed conditions; does not establish production effectiveness.
02 — Cross-agent evidence continuity	Gaps and inconsistencies across connected workflow stages	Explores traceability and control dependencies; scenarios informed detector design.
03 — AI measurement blind spot	Selective observation and monitoring bias	Demonstrates why observed-case metrics may not represent population risk; established auditing methods are relevant.
04 — Measurement paradox	Deteriorating underlying outcomes alongside improving observed metrics	Conventional stratified auditing outperformed the proposed trace-informed variant in this simulation.
05 — Unobservable risk	Missing potential outcomes and selection bias	Existing inverse-propensity methods recover the target under strong simulated randomisation assumptions.
06 — Cost of uncertainty	Risk limits, audit evidence and conservative deployment rules	Overly conservative rules can prevent unsafe approvals while rejecting many acceptable deployments.


Each experiment should be read alongside its individual methodology and limitations. Results are conditional on the simulated data-generating processes.
Selected findings
Experiment 04 — When monitoring moves in the wrong direction
Across two constructed periods, the experiment generated the following mean error rates:
Measure	Earlier period	Later period	Change
Simulator-known underlying error	7.02%	14.36%	+7.34 percentage points
Error among reviewed cases	6.80%	6.19%	−0.61 percentage points


The observed-only metric suggested improvement while the underlying synthetic outcome worsened. This is a constructed counterexample, not evidence that the same pattern exists at Citi or any other bank.
A further important finding: the proposed trace-stratified audit did not outperform conventional stratification in this setup. Later-period mean absolute error was approximately 1.59 percentage points for trace-stratified auditing versus 1.43 for the conventional observed-feature stratified audit. Reporting this negative result is central to the project's research integrity.
Experiment 05 — When the outcome is not observable
In a hypothetical onboarding scenario, the simulator generated potential outcomes for accepted and rejected applicants. In a real-world process, the adverse outcome if a rejected applicant had been onboarded would generally be counterfactual and unavailable.
- 6.66%: mean observed-only estimate in the deterministic selection regime.
- 17.39%: simulator-known population risk under the hypothetical intervention of onboarding everyone.
- 17.45%: known-propensity inverse-probability weighted estimate under a separate, artificially randomised exploration regime.
These values are not all measured under the same observation regime. The weighted estimator's success relies on known propensities and other assumptions deliberately guaranteed by the simulator. It is an established statistical method, not a newly validated FAIR-AI estimator. Randomly onboarding real applicants is not recommended by this research and may be impermissible.
Experiment 06 — The cost of being conservative
In the acceptable baseline simulation, a conservative upper-bound deployment rule approved 4 of 100 cases and rejected 96 of 100 otherwise acceptable deployment decisions. In the deteriorated scenario, the same rule approved none. The result illustrates a trade-off: stronger caution may reduce unsafe approvals while creating substantial missed opportunities.
These are outcomes of illustrative simulation rules, thresholds and cost assumptions—not measured bank losses, customer harms or compliance violations.
Proposed concept: Evidence Readiness Gate
The Evidence Readiness Gate is a research concept for structuring the questions that precede an AI deployment or scale-up decision. It is not a certified standard, proprietary validated estimator, or substitute for an institution's model risk and compliance processes.
Stage	Governance question	Illustrative evidence
1. Define the decision	What is the system intended to do, and what risk threshold applies?	Use case, intended population, owners, risk appetite and decision criteria.
2. Map observability	Which decisions and outcomes are visible, delayed, missing or counterfactual?	Selection rules, outcome maturity, missingness map and lineage.
3. Challenge measurement	Could the system's decisions bias the metrics used to judge it?	Denominators, cohort comparisons, sampling analysis and sensitivity checks.
4. Test independent evidence	What can independent review or established estimators identify?	Audit design, estimator assumptions, uncertainty ranges and alternative explanations.
5. Record a decision	Is the evidence adequate to approve, limit, defer or escalate deployment?	Explicit decision rationale, limitations, accountable owner and escalation path.
6. Reassess after deployment	What would trigger renewed testing or intervention?	Drift indicators, missing-label rates, monitoring thresholds and review cadence.


Important distinction: The experiments motivate these questions; they do not empirically validate the gate as an effective governance intervention.
Repository structure
.
├── README.md
├── RESEARCH_INTEGRITY.md
├── requirements.txt
├── .gitignore
└── experiments/
    ├── experiment_01/
    │   ├── run_experiment.py
    │   ├── README.md
    │   ├── control_register.csv
    │   └── results/
    ├── experiment_02/
    │   ├── run_experiment_02.py
    │   └── experiment_02/       # Generated synthetic outputs
    ├── experiment_03/
    │   ├── experiment.py
    │   ├── README.md
    │   ├── RESEARCH_REPORT.md
    │   ├── summary.csv
    │   └── replicate_results.csv
    ├── experiment_04/
    │   ├── experiment.py
    │   ├── README.md
    │   ├── RESEARCH_REPORT.md
    │   ├── summary.csv
    │   └── replicate_results.csv
    ├── experiment_05/
    │   └── ...                  # Script, report and synthetic results
    └── experiment_06/
        └── ...                  # Script, report and synthetic results
The tree reflects the existing research archive. Do not assume additional folders or documents exist until they are uploaded.
Reproduce the experiments
Requirements: Python 3.10 or newer; dependencies listed in [`requirements.txt`](requirements.txt).
From the repository root:
# Create an isolated Python environment
python -m venv .venv

# Activate it on macOS/Linux
source .venv/bin/activate

# Or activate it on Windows (PowerShell)
# .venv\Scripts\Activate.ps1

# Install dependencies
python -m pip install -r requirements.txt

# Run each experiment
python experiments/experiment_01/run_experiment.py
python experiments/experiment_02/run_experiment_02.py
python experiments/experiment_03/experiment.py
python experiments/experiment_04/experiment.py
python experiments/experiment_05/experiment.py
python experiments/experiment_06/experiment.py
Before rerunning: Some scripts write outputs alongside their source files and may overwrite bundled CSV or report artefacts. Use a separate working copy or branch if you need to preserve the archived outputs unchanged.
For Experiments 03–06, inspect RESEARCH_REPORT.md, summary.csv and replicate_results.csv. For Experiment 01, inspect its results/ folder; for Experiment 02, inspect its generated experiment_02/ output folder. The exact interpretation of a number depends on the experiment's population, observation regime, estimator and assumptions.
Reproducibility means that code and simulated outputs can be inspected and rerun. It does not imply independent peer review, external validation or generalisability to live financial institutions.
Methodology and research integrity
The project uses synthetic data-generating processes and compares observed metrics with quantities known to the simulator. Methods include rule-based control checks, statistical simulation, independent audit sampling, stratification, inverse-propensity weighting, partial-identification bounds and conservative risk-limit decisions.
Several limitations are essential:
- Synthetic ground truth: A simulator can observe quantities that a real bank cannot, including some counterfactual outcomes.
- Constructed selection effects: Scenarios are designed to test failure modes; they do not estimate how common those failures are in production.
- Strong assumptions: Some estimators rely on known selection probabilities, label maturity or independent sampling that may be unavailable in practice.
- No demonstrated novelty: Successful comparators often use established statistical methods. The project does not claim to have invented them.
- Negative results matter: Experiment 04 did not establish superiority of the trace-informed audit, and Experiment 06 exposed the opportunity cost of excessive conservatism.
- No institutional validation: There has been no testing against Citi systems or any other institution's live models, customer outcomes or operational controls.
- No legal compliance certification: The simulations do not establish conformity with the EU AI Act, GDPR, banking supervisory expectations or any other legal or regulatory framework.
Relevance to AI governance in financial services
The research is intended to support discussion across AI governance, model risk management, independent validation, operational risk and assurance functions. It highlights why a reported metric needs an accompanying account of who was measured, who was excluded, how outcomes became observable and which uncertainties remain.
Potential applications for future investigation include AI-assisted onboarding, transaction monitoring, decision-support systems and multi-stage AI workflows. These are research directions, not claims that the experiments validate controls for those applications.
Next research priorities
1. Test findings across alternative data-generating processes and unseen scenarios.
2. Model delayed, disputed and systematically missing outcomes.
3. Benchmark against stronger established audit and estimation methods.
4. Investigate audit budget, operational cost, fairness implications and risk appetite jointly.
5. Define measurable evidence-readiness criteria and falsifiable validation hypotheses.
6. Seek independent methodological review before any claim of real-world effectiveness.
Author and attribution
Uju
Founder, Voidborn Tech
Independent applied research in AI governance, risk analytics and evidence-based assurance.
Licence and reuse
No open-source licence is currently granted. Unless and until a licence is added, publication on GitHub does not by itself grant permission to copy, modify or redistribute the code. Contact the repository owner regarding reuse or collaboration.
