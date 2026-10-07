# Research and import workflow

Next Chapter lets users prepare job research with their preferred AI assistant and bring the result into their private workspace. The app supplies a reusable prompt, a portable skill and a blank template from the import screen.

The workflow is implemented today. It does not require an AI API key in Next Chapter or connect the app directly to a model provider.

## From priorities to a workbook

1. **Choose the assistant.** Copy the research prompt or download the skill from the import guide.
2. **Supply context deliberately.** Share the resume or summary you choose, target roles and domains, seniority, location preferences and constraints.
3. **Agree on priorities.** Separate hard constraints from preferences and define weights and scoring anchors before research begins.
4. **Research from evidence.** Prefer employer career pages and official applicant-tracking pages. Keep source URLs, uncertainty and actual verification dates.
5. **Generate a compatible workbook.** Use stable source IDs, the supported columns and plain cell values.
6. **Review in Next Chapter.** Select the file, inspect validation and preview information, confirm the signed-in destination account, choose an import mode, then confirm the import.

The external assistant receives only the information the user shares with it. Next Chapter's import guide does not send a resume or workspace data to an AI service.

## Two scores, two questions

| Score | Meaning | Required supporting reasoning |
| --- | --- | --- |
| Resume fit /100 | How the supplied resume aligns with the role's stated requirements and responsibilities | Matched evidence, gaps and the agreed fit rubric |
| Priority /100 | How attractive the role is under the user's own preferences | Criterion ratings, weights, evidence and hard-constraint outcomes |

The guide asks whether resume fit should contribute to priority rather than assuming it should. A priority calculation uses criterion ratings from 0–100 and weights totaling 100. For example, weights of 60 and 40 with ratings of 80 and 50 produce a priority score of 68.

Insufficient evidence should leave a score blank. Unknown criteria must not silently disappear from the calculation. A valid zero remains a zero. These scores are rubric-based assessments, not probabilities of getting an offer.

## Import contract

The standard format is an unencrypted `.xlsx` workbook with an **Applications** worksheet and, optionally, an **Instructions** worksheet. Headers belong in row 1; one posting occupies each subsequent data row.

| Field group | Supported columns |
| --- | --- |
| Identity | Source ID, Company, Role |
| Posting | Application URL, Location, Work arrangement, Office days per week, Role state |
| Personal tracking | Application status, Applied date, Follow-up date, Notes |
| Research | Research snapshot date, Last verified date, Research notes |
| Scores | Resume fit /100, Priority /100 |

Source ID, Company, Role and Research snapshot date are required. Optional fields may be blank; older supported templates without the score columns remain valid. Downloading the current in-app template avoids guessing header names or order.

The parser checks the container, supported worksheets and columns, unique identifiers, required values, field lengths, calendar dates, enumerated values and numeric bounds. Scores must be numeric 0–100 values. Formula cells, percentage-formatted scores, macros and external workbook links are not part of the standard template contract.

The guide requires a real Excel file. Renaming a CSV or text file to `.xlsx` does not produce a valid workbook. When an assistant cannot create a file, its tab-separated output can be pasted as values into the blank template and saved as Excel before import.

## Preserve identity and personal work

Use a stable Source ID for each posting and preserve it across research refreshes. A row number is not a stable identity.

By default, import skips records already matched in the account. The research-refresh option updates eligible job research while preserving the user's existing status, notes and application dates. Older research snapshots cannot replace newer ones.

Newly researched opportunities start as **Saved** unless the user explicitly supplies another personal status. An imported status is an initial observation; it does not create a fictional sequence of prior application stages.

## Compatibility and quality are separate checks

A file can have valid columns and still contain weak research. The preview verifies what the importer can accept. Evidence quality requires a separate review of listing URLs, claims, verification dates and scoring rationale.

The proposed Python and MLflow work in the [roadmap](roadmap.md) will make those checks more repeatable. It will evaluate supplied outputs or controlled experiments; it will not automatically observe users' external AI conversations.
