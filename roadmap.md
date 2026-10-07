# Next Chapter roadmap

This roadmap separates current application behavior from proposed work. It does not represent a release schedule or a commitment to specific dates.

## Available today

- A published application with Google and email/password sign-in.
- Private application tracking, follow-up dates and expandable notes.
- Imported resume-fit and priority scores with sorting and local search.
- Browser-validated workbook imports, stable source matching and research refresh.
- A reusable external-AI research prompt, portable skill and import template.
- A home-screen Sankey and tables based on recorded status history.
- Manual and automatic backups, JSON download/upload and confirmed restore.
- Compact cards, progressive loading, light/dark themes and a fictional demo.

## Proposed next steps

| Step | Purpose | Evidence of progress |
| --- | --- | --- |
| Small beta feedback round | Find confusing steps and failures in real use | Recorded feedback on first import, search, score interpretation and recovery |
| Python dataset validator | Check generated workbook structure consistently before import | A repeatable report for required fields, dates, duplicate IDs and score bounds |
| Research-quality benchmark | Evaluate more than file compatibility | A fixed sample set with source evidence, expected checks and human review |
| MLflow experiment tracking | Compare prompt and model versions over repeated runs | Versioned inputs, outputs, scoring criteria and evaluation results |
| Optional Databricks learning project | Explore shared evaluation workflows with synthetic data | Reproducible sample experiments, separate from the application's private workspace |

## Evaluation approach

Start with a small, fixed set of synthetic or appropriately shareable job examples. Record the prompt version, input context, model information when available and generated workbook. Check structural validity separately from factual support and the consistency of score reasoning.

Externally generated files can be evaluated after generation. Latency, token use and cost require an instrumented model call or provider-supplied records; they cannot be reliably recovered from a workbook alone.

Python, MLflow and Databricks are not required to run the current app. Any future integration should preserve the existing separation between user-directed research and private application tracking.
