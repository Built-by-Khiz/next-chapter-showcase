# Next Chapter product brief

## Problem

Job seekers collect opportunities faster than they can evaluate and act on them. Research may describe the role well, but personal fit, priorities, application status and follow-up context often live in different places. A refreshed spreadsheet can also overwrite personal work or imply that a researched role has already been applied to.

Next Chapter connects research to a private, ongoing application workflow.

## Intended users

- Job seekers managing opportunities across companies and job boards.
- People using spreadsheets or an AI assistant to research and rank potential roles.
- Applicants who want an understandable record of their actions and a way to recover earlier work.

The current product is organized around individual accounts. Shared recruiter pipelines and team collaboration are outside the current scope.

## Product goals

1. Help a user identify which role to consider next using their own evidence and priorities.
2. Bring structured research into the tracker without losing personal application work.
3. Make changes in application status visible without inventing historical activity.
4. Give users control over imports, backups and restores.
5. Let users choose the AI assistant they use for research.

## Core user journeys

| Journey | User outcome | Current behavior |
| --- | --- | --- |
| Explore | Understand the product before creating an account | A fictional demo runs in browser memory. |
| Start a workspace | Save personal application records | Google or email/password sign-in opens an account-specific workspace. |
| Add a role | Capture an opportunity quickly | Company and role are required; additional details are optional. |
| Bring in research | Turn a workbook into tracked opportunities | Browser validation, preview, destination confirmation and batched import. |
| Decide what comes next | Find suitable opportunities | Imported fit/priority scores, sortable headers, text/date/score search and status filtering. |
| Track activity | Keep the current stage and follow-up context | Application editing plus a chart of recorded status observations. |
| Recover work | Return to a saved state deliberately | Named backups, JSON download/upload, restore preview and a safety copy. |

## Three separate concepts

| Concept | Question it answers | Examples |
| --- | --- | --- |
| Job availability | What is known about the posting? | Open, Unverified, Closed, Archived |
| Application status | Where is the user in their own process? | Saved, Applied, Interviewing, Offer, Accepted, Rejected, Withdrawn |
| Fit and priority | How does this opportunity compare under the user's rubric? | Resume fit 88/100; priority 91/100 |

None of these concepts is inferred from the others. A high-priority role may be unverified. An open role may still be Saved. Researching a role does not record an application submission.

## Interaction decisions

### Compact cards with detail on demand

Company, role, location, dates and scores remain visible in the list. Notes appear when a card expands. The status badge sits beside compact icon actions. Shared score headers support sorting without repeating their labels in every card.

The list starts with 50 matching roles and progressively reveals another 50. Sorting and filtering apply to the complete loaded workspace before rows are revealed.

### Familiar search with predictable behavior

The search box supports terms, quoted phrases, field filters, dates and numeric score comparisons. It interprets a defined query grammar locally. Multiple conditions narrow the result together; it does not promise to understand arbitrary conversation.

### Progress on the workspace home screen

The Sankey chart places recorded activity beside the application list. A starting observation describes what was known when tracking began. Later status changes create the recorded journey. When no changes exist, a current-status view remains useful.

### Review before importing or restoring

Users review workbook validation and the destination account before importing. Existing records are skipped by default. An explicit research-refresh option preserves existing personal fields.

Restoring a backup is a separate action from uploading one. The preview compares record counts, explains the replacement and requires confirmation. A safety copy preserves the prior workspace.

## Scope boundaries

The current app tracks follow-up dates but does not deliver email or calendar reminders. It does not submit job applications, scrape job sites in the background, calculate fit scores inside the app, or infer the probability of receiving an offer.

The external research guide helps users prepare a compatible dataset. File validation establishes structural compatibility; the user still needs to assess the quality of the research and scores.

## Early validation

Initial checks on the published app covered sign-in and session refresh, separate account workspaces, a fictional three-role import with expected scores, and backup download/upload/restore behavior. These are functional checks, not a security certification or a measure of job-search outcomes.

The next feedback round should examine time to first useful import, confusing search or score behavior, import/restore failures, and whether people return to update their applications. These are proposed measures; no usage or outcome metrics are claimed here.
