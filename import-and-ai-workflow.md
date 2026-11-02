# Research and import workflow

Next Chapter is a job application tracker that can turn externally researched workbooks into an ongoing private workspace. Users choose their own AI assistant. The app provides a reusable research prompt, a portable skill and a blank Excel template; it does not send a resume to a model or require an AI API key.

This document describes the latest implementation. See the [README](README.md) for the publication status of the latest interface changes.

## Start with a file or build one with an assistant

The import page opens with **File Import**. A headline button opens **Build a workbook with your own AI assistant** as a separate view. Its **File Import** button returns to the upload flow. Switching views preserves the current file and choices; switching is disabled while an operation needs that state.

The research guide starts with the user's own context:

1. Reuse a supplied resume, current application export and already-confirmed preferences. Ask only for material missing information.
2. Confirm target roles, domains, seniority, employer preferences, location, work arrangement and other constraints. Optional experience screening requires an explicit cutoff and interpretation; no experience cutoff is enabled by default.
3. Agree on the scoring model and the task's scope: new discovery, availability refresh, experience review or rescoring.
4. Research exact postings from employer or official applicant-tracking sources, preserving stable identities and dated evidence.
5. Produce the supported workbook, check its structure and report evidence gaps separately from file compatibility.

The guide is reusable. It does not embed one user's employer exclusions, resume, geography or experience rule. The user controls which personal information goes to the external assistant.

## The five-step import wizard

| Step        | What the user does                                                                                            |
| ----------- | ------------------------------------------------------------------------------------------------------------- |
| **File**    | Select an unencrypted `.xlsx` file. Parsing and validation run in a browser worker.                           |
| **Review**  | Read aggregate counts and warnings. Expand the paginated record preview only when needed.                     |
| **Updates** | Keep the default **Skip existing records**, or choose **Refresh existing research**.                          |
| **Cleanup** | Explicitly choose to keep current status categories or review removal from selected categories.               |
| **Import**  | Review the destination account and chosen settings, confirm, then follow progress beside the active controls. |

File preview does not upload the workbook. Confirmed import stages normalized records on the server. If the live deletion plan contains matched records, the app presents its count and preview and requires a separate deletion confirmation before changes begin. A complete pre-import safety backup must succeed first.

Back and Continue controls keep each decision in the current step. Aggregate information remains visible without requiring the user to inspect every row or scroll through the entire dataset.

## Export current applications before the next research cycle

**Export applications**, available in the workspace's `...` menu, downloads application-context JSON for a future research prompt. It includes all current applications regardless of list filters, along with current statuses, personal notes, dates, imported research, scores and matching information. It does not include status-transition history.

This context file is distinct from both an import workbook and a restorable backup. It can contain personal information, so the user chooses whether to share it with an external assistant. The guide also understands a supplied raw Next Chapter backup, but neither JSON format can be selected as an Excel import file.

| Export matching marker | How future research should use it                                                                                       |
| ---------------------- | ----------------------------------------------------------------------------------------------------------------------- |
| `existing_source_id`   | Preserve the exact `source_id` as the workbook's **Source ID**.                                                         |
| `reference_only`       | Use the record as context and flag overlaps. Do not invent an ID that implies it can safely update the existing record. |

Manual applications generally fall into the second group. The importer does not merge them with new workbook rows by company, title or URL similarity. A new Source ID represents a different imported record, even when the visible details resemble an existing one.

## Add missing roles and refresh retained research

Stable Source IDs let the importer distinguish new records from existing imported records in the same account.

- **New Source ID:** create a job and its initial application. Newly researched roles normally start as Saved.
- **Existing Source ID with Skip existing records:** preserve its current job research and personal tracking.
- **Existing Source ID with Refresh existing research:** accept eligible same-date or newer research while preserving existing personal status, notes, applied date and follow-up date.

Research refresh replaces the complete imported job details and research object. It is not a merge of nonblank fields. The next workbook must carry forward unchanged URLs, locations, arrangement, verification details, scores and relevant evidence; a blank can clear a previous imported value. Older research for a retained role blocks refresh confirmation rather than silently skipping that row. Review the workbook or use Skip existing records; dates must not be fabricated to bypass that rule.

If the imported research fields already match, the database skips the unnecessary update. A refreshed snapshot date is itself a research-field change.

Personal statuses are initial values for newly created applications. A workbook does not reclassify an existing application or apply a company-wide status decision. In the app, choosing **Not interested in the company** asks whether it applies to the chosen role or roles, or all current roles from the matching company. An export's individual status alone does not establish that original scope or a permanent employer exclusion.

## Explicit deletion annotations and optional cleanup

Deletion has two distinct inputs:

| Input                               | Meaning                                                                                                                                  |
| ----------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------- |
| Workbook **Import action = Delete** | Request removal of the existing imported record with that exact Source ID, supported by a **Deletion reason**.                           |
| Selected current status categories  | Add existing workspace records in those categories to the live cleanup plan, including manual roles and roles omitted from the workbook. |

For new imports, blank **Import action** always means **Upsert**, regardless of personal status. A Closed or Archived posting state, a declined personal status, an omitted row or a low score does not by itself request deletion.

The Cleanup step has no preselected choice or category. Its supported categories are:

- Role no longer available
- Not interested in domain
- Not interested in the company
- Seniority too high
- Not interested (reason not set), retained for legacy records

Choosing to keep these categories still permits explicit workbook Delete requests. Explicit deletions apply in both Skip existing and Refresh existing modes. The server combines both inputs into one plan, deduplicated by live record ID. It includes matching current records hidden by filters or beyond the visible cards. The final matched count can differ from the workbook's offline deletion count.

Unknown Delete IDs are reported without creating a job. Records selected for cleanup are suppressed from upsert during that import so they are not immediately recreated. Other omitted records remain. Deleting a job removes its application, notes, dates and status history; the pre-import backup is the recovery copy. A changed workspace can invalidate the review and require fresh confirmation.

The research prompt asks for Delete annotations when current exact-posting evidence establishes closure, or a separate explicit user removal instruction or authorized screening rule applies. A timeout, CAPTCHA, lone broken URL, incomplete job-board search or uncertain application gate is not enough. Such uncertainty remains Unverified unless another independently authorized removal reason applies. The importer validates annotations and matching; it does not visit employer websites to verify their truth.

Existing unfinished imports retain their original protocol rules. To use the latest optional-cleanup behavior instead, cancel an older run and start a new one. Cancellation does not undo batches that already committed.

## Two scores, with actionability kept separate

Scores are generated outside the app and imported as values. Next Chapter displays, searches and sorts them; it does not calculate or train them at runtime.

| Score               | Current prompt's default approach                                                                                                                       |
| ------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Resume fit /100** | Assess supplied resume evidence against domain, function, seniority, technical requirements and eligible location, weighted 30%, 25%, 20%, 15% and 10%. |
| **Priority /100**   | Apply the user-confirmed preference model `NC-MCDA-2026-10-29-v2`, or an explicitly agreed and separately versioned custom model.                       |

The default priority model uses four 0–100 utilities:

```text
Priority = (0.30 × fit
          + 0.40 × office preference
          + 0.20 × company potential
          + 0.10 × growth relative to headcount)
          × monetization preference multiplier
          × engineering preference multiplier
```

The unchanged model gives office-preference contributions of 16, 40, 32, 24, 8 and 0 points for 0, 1, 2, 3, 4 and 5–7 office days respectively. Company potential uses supported revenue growth, comparable market or valuation momentum, financing and liquidity evidence. The growth/headcount component uses an explicit headcount factor and a reduced-weight market proxy when recognized revenue growth is missing. These are preference rules, not forecasts of equity proceeds or company success.

The default reductions are ×0.5 for primarily billing/pricing/monetization roles and ×0.75 for substantial mandatory professional software engineering or sustained production implementation. They compound when both apply. The assistant asks the user to confirm or customize the model rather than treating these preferences as universal.

The full downloadable prompt defines component arithmetic, evidence rules, missing-input scenarios and sensitivity checks. It rounds the final result once to one decimal, half up. It does not force a normal distribution, percentile ranking or minimum-score floor.

Unknown raw facts and unassessable raw resume-fit scores stay blank. For new scoring and authorized rescoring, the default priority calculation uses explicitly labeled estimated utilities for missing inputs and reports evidence coverage and missing-input ranges. Estimated inputs are not invented facts, and those ranges are not statistical confidence intervals. Genuine calculated zero remains valid.

An availability-only or experience-only refresh preserves the supplied scores and their rationale, including historical blanks, unless the user separately authorizes rescoring. Availability, eligibility, personal interest and priority remain separate: a closed or declined role can retain a score without being recommended or reactivated.

## Workbook contract

Use the current downloadable template. The standard workbook has an **Applications** sheet and optionally an **Instructions** sheet. Headers are exact and case-sensitive in row 1, with one unique posting per data row. The optional Instructions sheet is not imported.

| Field group       | Exact supported columns                                                       |
| ----------------- | ----------------------------------------------------------------------------- |
| Identity          | Source ID, Company, Role                                                      |
| Posting           | Application URL, Location, Work arrangement, Office days per week, Role state |
| Personal tracking | Application status, Applied date, Follow-up date, Notes                       |
| Research          | Research snapshot date, Last verified date, Research notes                    |
| Scores            | Resume fit /100, Priority /100                                                |
| Deletion          | Import action, Deletion reason                                                |

**Source ID**, **Company**, **Role** and **Research snapshot date** are required headers and values, including on deletion rows. The other columns are optional; older supported templates without deletion columns or scores remain accepted. There can be only one row per Source ID.

Key rules:

- Source IDs are permanent text, up to 250 characters. Company and Role allow 200 and 250 characters respectively.
- Dates use valid `YYYY-MM-DD` values. Unknown optional dates remain blank.
- Work arrangement accepts Remote, Hybrid, Onsite or Unknown; blank means Unknown. Office days are numbers from 0 to 7, with at most two decimal places.
- Role state accepts Open, Unverified, Closed or Archived; blank means Unverified. It describes posting availability.
- Application status accepts Saved, Applied, Interviewing, Offer, Accepted, Rejected, Withdrawn, Not interested in domain, Not interested in the company, Seniority too high or Role no longer available. Legacy unspecified Not interested remains readable for older records. Blank means Saved.
- Scores are optional plain numeric cells from 0 to 100, including decimals. Do not use percentages, formulas or text such as `85/100`.
- Import action accepts Upsert or Delete. A deletion reason can contain up to 2,000 characters. Provide a specific reason for every Delete request; a legacy default remains accepted for an explicitly deleted Role no longer available row.
- Notes are personal tracking context, up to 20,000 characters. Research notes contain concise sources, scoring rationale and uncertainty.
- Use plain values, not formulas. The standard template does not accept macros or external workbook links. Renaming CSV to `.xlsx` does not create a valid workbook.

Supported legacy Superset workbooks follow their own recognized schema. New AI-generated datasets should use the current standard template rather than depending on legacy layout detection.

| Limit                     | Current boundary                                          |
| ------------------------- | --------------------------------------------------------- |
| Workbook                  | 10 MiB compressed, 64 MiB expanded, 1–5,000 data rows     |
| Normalized import payload | 32 MiB                                                    |
| Research per record       | 64 KiB                                                    |
| Resulting workspace       | 5,000 applications and 25,000 status-history records      |
| Workspace and safety copy | 32 MiB, with capacity under the account's 50-backup limit |

Splitting a file cannot bypass total workspace limits. The live app checks account capacity after matching and planned cleanup.

## Recovery and evidence quality

Imports use durable server checkpoints, exact run identities and bounded staging/processing requests. Group sizes adapt to response time and shrink after recoverable failures. A lost response triggers a saved-progress check before the next request, rather than assuming the previous write failed. Users can resume an interrupted run; if staging is incomplete, the original matching workbook is needed again.

Each processing batch is transactional, but the entire workbook is not one transaction. Confirmed earlier work remains after cancellation or a later failure. This design reduces repeated work and duplicate writes without promising that networks or database requests can never time out.

Structural validation checks whether the file can be processed. It does not establish that a job is still open, that a deletion reason is correct or that scoring arithmetic is justified. The prompt asks the research assistant to check these separately and report what was actually verified. Proposed Python validation and MLflow evaluation remain separate from the current app runtime; see the [roadmap](roadmap.md).
