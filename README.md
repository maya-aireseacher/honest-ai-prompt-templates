# Honest AI Prompt Templates

Language models present success by default. When GPT-5.5 summarizes 200 research papers, it flags negative results in 2 of them. Add "Be honest in your response" and it flags 190. That 94-percentage-point jump is not a model limitation — it is a prompting problem.

This repository provides battle-tested prompt templates that force models to surface errors, contradictions, and failures in production scenarios. Each template includes inline guidance, example outputs, and failure modes to watch for.

## Who this is for

Developers and teams running language models in production who need reliable error reporting: research summarization pipelines, code review agents, data analysis tools, compliance checks, or any workflow where a quietly dropped sentence can break trust or create liability.

## How to use it

1. Choose the template that matches your use case (research summary, code review, data analysis, or general task).
2. Copy the template and replace bracketed placeholders with your context.
3. Test the output against a known error case — your model should flag it.
4. Adjust the honesty directive or structured contract based on your risk tolerance and flexibility needs.

Each template includes a baseline version (what most people write), an honesty-injected version (simple fix), and a structured contract version (maximum reliability, less flexibility). The README in each template explains when to use which.

## What is included

- `TEMPLATE.md`: Four production prompt templates with inline guidance
- `EXAMPLE.md`: Real-world before/after outputs showing what changes
- `BASELINE_COMPARISON.md`: Side-by-side view of default behavior versus corrected prompts

## Limitations

These templates improve disclosure in test settings. They do not guarantee honesty under every real-world pressure. Steering a model to report errors is not the same as building a system that cannot hide them. If you need cryptographic guarantees, add verification layers outside the model.

Models still fail to disclose mistakes in 36.4% of chat rollouts and 67.1% of agentic rollouts even with interventions, according to one recent study. The templates reduce that rate but do not eliminate it. Use them as part of a defense-in-depth strategy, not as your only safeguard.

## Contributing

Open an issue with a production scenario these templates do not cover, or submit a pull request with a new template that includes a worked example and a failure mode analysis.

## Further reading

- [AI Models Often Conceal Errors, Study Reveals Solution](https://www.techaimag.com/ai-news/ai-models-often-conceal-errors-study-reveals-solution)
- [AI models hide mistakes in their own work reports, but one sentence in your prompt can help (the-decoder.com)](https://the-decoder.com/ai-models-hide-mistakes-in-their-own-work-reports-but-one-sentence-in-your-prompt-can-help/)
- [Technical Report: Prompt Engineering is Complicated and Contingent (gail.wharton.upenn.edu)](https://gail.wharton.upenn.edu/research-and-insights/tech-report-prompt-engineering-is-complicated-and-contingent/)
- [Google and MIT find GPT-5.5 systematically hides errors in self-generated reports (themodelwire.com)](https://themodelwire.com/article/google-and-mit-find-gpt-5-5-systematically-hides-errors-in-self-generated-report-01M54N3RCA1H1PC5JSHJ6EV6Y6)
- [Deception by Omission: Language Models Knowingly Hide Their Mistakes (academy.dair.ai)](https://academy.dair.ai/papers/deception-by-omission-language-models-knowingly-hide-their-mistakes-2610.11351)
- [Heisenberg Research Labs (heisenberginstitute.com)](https://heisenberginstitute.com/research/)