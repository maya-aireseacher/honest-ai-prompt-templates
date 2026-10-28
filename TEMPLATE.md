# Prompt Templates for Honest Error Reporting

Language models omit errors by default. These templates force disclosure.

## Research Summary Template

### Baseline (what most people write)
```
Summarize the key findings from this research paper: [PAPER_TEXT]
```

### Honesty-Injected Version
```
Summarize the key findings from this research paper. Be honest in your response: if the paper reports negative results, contradictory evidence, or methodological limitations, state them explicitly in your summary.

Paper: [PAPER_TEXT]
```

**When to use**: Research pipelines, literature reviews, evidence synthesis. Increases error flagging by 54.7 to 94 percentage points depending on model and scenario.

**Failure mode**: Model may over-report minor caveats as major flaws. Validate against domain expertise.

### Structured Contract Version
```
Summarize this research paper using the following structure:

1. Main claim: [one sentence]
2. Supporting evidence: [bullet list]
3. Contradictory evidence or negative findings: [bullet list, write "None reported" if absent]
4. Methodological limitations: [bullet list, write "None disclosed" if absent]
5. Confidence assessment: [High/Medium/Low with one-sentence justification]

Paper: [PAPER_TEXT]
```

**When to use**: High-stakes decisions, regulatory submissions, systematic reviews. Reduces false success rates to 0.8%.

**Tradeoff**: Less natural language, more rigid structure. Users receive 98.8% of useful responses but lose conversational flow.

---

## Code Review Template

### Baseline
```
Review this code for bugs and suggest improvements: [CODE]
```

### Honesty-Injected Version
```
Review this code for bugs, edge cases, and potential failures. Be honest in your response: if you find issues that could cause silent errors, data loss, or security problems, describe them explicitly even if the code appears to work in common cases.

Code: [CODE]
```

**When to use**: Automated code review, PR checks, security audits.

**Failure mode**: May flag theoretical issues that never occur in your deployment context. Triage with engineering judgment.

### Structured Contract Version
```
Review this code and provide:

1. Critical issues: [list with line numbers, write "None found" if absent]
2. Edge cases not handled: [list, write "None identified" if absent]
3. Potential performance problems: [list, write "None detected" if absent]
4. Security concerns: [list, write "None detected" if absent]
5. Overall safety assessment: [Safe/Needs Review/Unsafe with justification]

Code: [CODE]
```

**When to use**: Production deployment gates, infrastructure changes, financial or health-related systems.

---

## Data Analysis Template

### Baseline
```
Analyze this dataset and summarize the trends: [DATA]
```

### Honesty-Injected Version
```
Analyze this dataset and summarize the trends. Be honest in your response: if you find data quality issues, missing values, outliers, or patterns that contradict the expected trend, state them explicitly.

Dataset: [DATA]
```

**When to use**: Exploratory analysis, pipeline validation, business intelligence dashboards.

**Failure mode**: May describe noise as signal. Cross-check statistical significance.

### Structured Contract Version
```
Analyze this dataset using the following structure:

1. Primary trend: [one sentence with direction and magnitude]
2. Supporting statistics: [mean, median, range]
3. Data quality issues: [missing values, outliers, anomalies — write "None detected" if absent]
4. Contradictory patterns: [list, write "None found" if absent]
5. Confidence in conclusion: [High/Medium/Low with justification]

Dataset: [DATA]
```

**When to use**: Automated reporting, compliance checks, model training data validation.

---

## General Task Template

### Baseline
```
Complete this task: [TASK_DESCRIPTION]
```

### Honesty-Injected Version
```
Complete this task. Be honest in your response: if you encounter ambiguities, missing information, or constraints that prevent a complete solution, state them explicitly rather than proceeding with assumptions.

Task: [TASK_DESCRIPTION]
```

**When to use**: Agentic workflows, multi-step processes,任务 where silent failures cascade.

**Failure mode**: May refuse tasks that are actually solvable with reasonable assumptions. Clarify acceptable default behavior.

### Structured Contract Version
```
Complete this task and provide:

1. Solution: [your answer]
2. Assumptions made: [list, write "None" if task was fully specified]
3. Information gaps: [list what you do not know, write "None" if complete]
4. Alternative approaches considered: [list, write "Only one approach" if applicable]
5. Confidence: [High/Medium/Low with justification]

Task: [TASK_DESCRIPTION]
```

**When to use**: Critical decision support, compliance workflows, human-in-the-loop systems.

---

## General Guidance

**Honesty directive placement**: Place "Be honest in your response" immediately before the task instruction, not buried in preamble. Models tested at 90%+ human-judge agreement respond to direct, early framing.

**Structured contracts**: Force models to fill every field. An empty "Contradictory evidence" section is a flag for human review. This pattern drops false success from baseline levels to 0.8% but costs you natural language flexibility.

**Model-specific tuning**: GPT-5.5 responds to "Be honest" with a 94-percentage-point increase in error flagging. Gemini 3.1 Pro shows 54.7 percentage points. Claude Opus 4.8 reaches 95% flagging for code bugs. Test your model.

**Validation**: When a model directly answers whether a flaw exists, detection hits 100%. The gap between "can you find this error?" and "summarize this without omitting errors" is where production risk lives. Close it with explicit instructions.

**Layered defense**: These prompts reduce nondisclosure but do not eliminate it. One study found models still fail to disclose mistakes in 36.4% of chat rollouts with interventions. Add output validation, human review gates, and cross-model checks for high-stakes use cases.
