# Next Chapter product brief

## Problem

Applicants collect opportunities faster than they can evaluate and act on them. Role research, personal priorities, application status, rejection emails and follow-up context often live in separate places. Updating a research spreadsheet can also erase personal decisions or make an old opportunity look new.

Next Chapter is a private job application tracker that keeps research useful throughout the application process. Users bring their own research, record decisions, refresh information deliberately and retain control over changes to their workspace.

## Intended users and goals

The app serves individuals managing applications across companies and job boards, including people who use spreadsheets or an external AI assistant to prepare research. Shared recruiter pipelines and team collaboration are outside the current scope.

The product aims to:

1. Make the next application decision easier with distinct resume-fit and priority scores.
2. Refresh research without overwriting existing personal statuses, notes or dates.
3. Reduce repeated work through quick status changes, multi-select updates and email review.
4. Keep larger imports recoverable through saved progress and explicit confirmation.
5. Show recorded progress without inventing earlier application stages.
6. Keep the interface focused on the current task, with detail available when requested.

## Core user journeys

| Journey                         | User outcome                                                 | Implemented behavior                                                                                                            |
| ------------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------- |
| Explore                         | Understand the product before creating an account            | A fictional demo runs in browser memory.                                                                                        |
| Start or recover access         | Open a private workspace                                     | Google or email/password sign-in, plus email-based password recovery.                                                           |
| Capture an opportunity          | Add a role without a workbook                                | Company and role are required; additional details are optional.                                                                 |
| Bring in research               | Add new opportunities and refresh existing research          | A guided workbook import with browser validation, review, update choices, cleanup choices and confirmation.                     |
| Decide what comes next          | Find and compare relevant opportunities                      | Imported fit/priority scores, sortable headers, supported text/date/score queries and status filtering.                         |
| Update tracking                 | Record a decision with little interruption                   | Click a status badge, edit a card, or select multiple applications for a status update.                                         |
| Process a rejection             | Connect an email to the right tracked role                   | A private forwarding address, suggested matches, explicit review and optional narrowly gated automatic rejection updates.       |
| Prepare the next research round | Give an external assistant current tracking context          | Export all current applications as JSON, including stable source identities, current statuses, notes and research.              |
| Recover or clear work           | Return to an earlier state or deliberately empty a workspace | Manual/automatic backups, JSON download/upload, restore preview, and a protected delete-all action with a required safety copy. |

## Three separate concepts

| Concept            | Question it answers                                       | Examples                                                                 |
| ------------------ | --------------------------------------------------------- | ------------------------------------------------------------------------ |
| Job availability   | What is known about the posting?                          | Open, Unverified, Closed, Archived                                       |
| Application status | Where is the user in their own process?                   | Saved, Applied, Interviewing, Rejected, or a reason for declining a role |
| Fit and priority   | How does the opportunity compare under the user's rubric? | Resume fit 88/100; priority 91/100                                       |

A high-priority role may be unverified. An open posting may still be Saved. Researching a role does not record an application submission. Scores come from imported research and are not probabilities of getting an offer.

The selectable application statuses are **Saved, Applied, Interviewing, Offer, Accepted, Rejected, Withdrawn, Not interested in domain, Not interested in the company, Seniority too high**, and **Role no longer available**. Older **Not interested (reason not set)** records remain readable and can be preserved during editing; new choices capture a more specific reason.

Choosing **Not interested in the company** asks whether the change applies only to the selected role or roles, or to all existing roles from the matching company or companies. The confirmation includes roles hidden by the current filter. Company matching ignores case and extra spaces; it does not infer subsidiaries or create a permanent rule for future imports.

## Interaction decisions

### Put actions before explanations

Add application and Import workbook remain visible on the workspace. Email updates, Export applications and Backups sit in the `...` menu. Info icons beside headings expand background guidance inline when clicked, with keyboard support. They do not rely on hover tooltips.

Action-specific instructions, errors and destructive-action confirmations remain visible when needed. Notes stay inside expanded cards. The import screen defaults to file selection; a headline button switches to the separate AI workbook guide and **File Import** returns to the wizard.

### Keep list work in context

Compact cards show company, role, location, dates, status and scores. Shared score headers sort the list. A superscript **NEW** marks applications created within the last seven days; editing a role or changing its research date does not make it new.

The list initially reveals 50 matching roles and reveals another 50 on scroll. Filtering and sorting operate on the loaded workspace before cards are revealed. Existing-card edits preserve the current search, filter, sort, revealed rows and scroll position during that workspace visit. This is not a promise of persistence across browser reloads.

Users can select loaded cards or all matching records for a bulk status update. A thin progress bar appears along the top of an updating card. Successful status changes briefly highlight the status badge with a checkmark; other successful edits highlight the card. If an update removes a card from the active filter, a nearby receipt explains the result.

### Show actual progress

The home-screen Sankey uses recorded status observations. Applications without an actual status change flow directly to **Current status**, skipping historical columns. Applications with changes use only their needed history steps, and the final state appears once in Current status. Same-status writes do not add a transition.

Initial observations do not recreate earlier stages. Ambiguous or incomplete histories remain explicit rather than being guessed. Exact-count tables support the visual chart.

### Make imports deliberate and recoverable

The wizard moves through **File → Review → Updates → Cleanup → Import**. Validation totals remain visible while the paginated record preview starts collapsed. The user can inspect rows, choose whether to skip existing records or refresh research, and then confirm the destination and import. Progress stays close to the active import controls.

Existing research refreshes preserve existing personal tracking fields. Explicit workbook deletion instructions are separate from optional cleanup of current unavailable, declined or over-senior roles. The app shows a deletion plan and requires additional confirmation before deletion. An absent workbook row is not a deletion instruction, and marking a role unavailable does not immediately delete it.

Imports use saved checkpoints and bounded processing groups. Interrupted work can resume from confirmed progress; some transient failures trigger smaller groups and checked retries. Earlier confirmed groups remain saved when an import is paused or cancelled. These controls reduce repeat work but do not promise that network or database timeouts can never occur.

### Let the user control email updates

Only messages delivered to the user's private forwarding address enter the email workflow. The app offers company/role suggestions for review and requires the user to check a selected application before confirming **Rejected**.

Optional automatic updates require verified forwarding, a configured trusted sender, sender authentication, a fresh explicit rejection, one unambiguous match, an eligible Applied or Interviewing record, and current-state checks. Manually forwarded messages require review. Uncertain messages remain reviewable, and irrelevant messages can be ignored or dismissed. See [Email updates](email-updates.md).

### Separate export, backup and deletion

Application export is context for future research, not an import workbook or a restorable backup. It includes all current applications regardless of list filters, but not historical status transitions. Users decide whether to share those personal notes and details with an external assistant.

Backups preserve roles, applications and recorded status history. Uploading a backup only saves a copy; restore is a separate reviewed operation. The Advanced delete-all action also requires review, typed confirmation and a successful safety copy. Account access, saved backups and automatic-backup preferences remain intact.

## Scope and evidence

The app does not submit job applications, scan a connected inbox, scrape job sites in the background, calculate fit scores inside the app, or deliver follow-up reminder emails. External AI research remains user directed. Structural workbook validation cannot prove that a job is still open or that its score is well supported.

The [README](README.md) identifies the version and publication boundary for these documents. Local checks cover application logic and desktop/mobile flows with fictional data; they are not a security certification or evidence of job-search outcomes. The [roadmap](roadmap.md) describes proposed evaluation work without claiming usage or outcome metrics.
