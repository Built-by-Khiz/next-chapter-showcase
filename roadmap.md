# Next Chapter roadmap

This roadmap separates implemented application behavior from proposed work. It is not a release schedule or a commitment to specific dates. See the [README](README.md) for the latest implementation and publication status.

## Implemented capabilities

| Area                  | Current implementation                                                                                                                                   |
| --------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Private workspace     | Google and email/password sign-in, password recovery, account-owned roles and applications.                                                              |
| Application decisions | Quick status changes, multi-select updates, specific decline reasons and an explicit company-wide scope choice.                                          |
| Focused interface     | Compact cards, expandable notes, click-to-expand help, an overflow menu, seven-day NEW tags and local update feedback.                                   |
| List navigation       | Score sorting, supported text/date/score filters, 50-card reveal increments, and preservation of list context while editing.                             |
| Research loop         | A reusable external-AI prompt and skill, a compatible template, and current-application JSON export for the next research round.                         |
| Guided imports        | File-first wizard, aggregate review, optional row details, stable source matching, research refresh, explicit deletion review and opt-in status cleanup. |
| Import recovery       | Saved run checkpoints, bounded batches, checked retries, smaller groups after transient failures, pause/resume and cancellation.                         |
| Email updates         | Private forwarding addresses, rejection detection, precise review suggestions and optional automatic updates under strict matching and sender checks.    |
| Progress              | A home-screen Sankey and exact counts based on actual recorded changes; unchanged roles go directly to current status.                                   |
| Workspace recovery    | Manual and scheduled backups, JSON download/upload, confirmed restore and protected delete-all with a safety copy.                                       |
| Exploration           | Light/dark themes and a fictional browser-only demo.                                                                                                     |

The implemented email workflow is documented in [Email updates](email-updates.md). The research, export, import and cleanup cycle is documented in [Research and import workflow](import-and-ai-workflow.md).

## Proposed next steps

| Step                                           | Purpose                                                                           | Evidence of progress                                                                                                                      |
| ---------------------------------------------- | --------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------- |
| Confirm deployment and gather focused feedback | Verify the latest UI in the published app and identify remaining friction         | Checks of wizard navigation, discoverable help, mobile layout, list context and actual-change chart paths, plus recorded user feedback    |
| Broaden recovery and scale testing             | Understand behavior near supported workspace/file limits and during interruptions | Repeatable cases for connection loss, lock contention, resume, cancellation and partial completion                                        |
| Expand email evaluation                        | Measure when matching or rejection detection needs human review                   | Synthetic or consented examples spanning shared recruiting senders, title variants, stale mail, correction messages and ambiguous matches |
| Python dataset validator                       | Check generated workbook structure before import with a repeatable tool           | Reports for required fields, date values, duplicate identities, score bounds and deletion annotations, aligned with the app's parser      |
| Research-quality benchmark                     | Evaluate more than file compatibility                                             | Fixed examples with source evidence, expected checks, scoring anchors and human review                                                    |
| MLflow experiment tracking                     | Compare prompt and model versions over repeated research runs                     | Versioned inputs, generated outputs, rubric versions and evaluation results                                                               |
| Optional Databricks learning project           | Explore shared evaluation workflows outside the app runtime                       | Reproducible experiments using synthetic or appropriately shareable data                                                                  |

## Evaluation approach

Use a fixed set of synthetic or appropriately shareable examples. Record the research prompt version, user preference profile, model information when available, generated workbook and validation results. Check structural compatibility separately from evidence quality and the consistency of score reasoning.

A useful test set should include repeated imports, unchanged duplicates, newer and older research, unknown scores, explicit deletion rows, status-based cleanup, and records that must remain untouched. Email evaluation should measure false automatic updates separately from missed suggestions. Ambiguous messages should remain reviewable rather than being forced into a match.

Externally generated files can be evaluated after generation. Latency, token use and cost require an instrumented model call or provider-supplied records; a workbook alone cannot reliably supply those measures. No usage, model-quality or job-outcome results are claimed here.

Python, MLflow and Databricks are proposed evaluation tools, not dependencies of the current app. Future experiments should preserve the separation between external research and private tracking, and should not ingest personal resumes, notes or emails without a deliberate data-sharing decision.
