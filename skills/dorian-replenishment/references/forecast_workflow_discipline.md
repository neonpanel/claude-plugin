# Forecast Workflow Discipline — Knowledge

## Rules for handling user-provided scenario names, labels, and formatting conventions

**What it is**

Rules for handling user-provided scenario names, labels, and formatting conventions. The agent must use the user's exact naming — never rename, relabel, or 'improve' user-provided identifiers. After commits, attribution must include the scenario name, what was saved, and who it's attributed to.

**Key objects**

- Scenario name
- User-provided label
- Attribution record
- Naming format

**How to help**

- Save this as scenario X
- Name this forecast Y
- What was the last scenario I saved?
- Show me the audit trail

**Clarifying questions**

- Agent renaming scenarios for 'clarity'
- Not preserving the user's exact formatting
- Missing attribution details after commit

## Commit Forecast Data

**Clarifying questions**

- What scenario name should this be saved under?
- Can you confirm the SKUs and date range included in this write?
- Should this overwrite the existing scenario or create a new version?

**How to help**

- Compare proposed adjustments against current plan and actuals
- Show the user the gap before proceeding
- Present a dry run of exactly what will be written
- Wait for explicit user approval before committing
- Commit to the live database
- Summarize what was saved: scenario name, data scope, attribution, timestamp

**Don't claim**

- Never commit forecast data without showing a dry run first
- Never rename or modify user-provided scenario names
- Never auto-save without explicit user approval

**Escalate when**

- Dry run reveals data discrepancies vs. actuals
- User requests a write but variant count doesn't match expectations

## The step-by-step discipline for writing forecast data: audit current plan against actuals, show the gap, dry run the proposed write, get explicit user approval, commit to the live database, and summarize the audit trail

**What it is**

The step-by-step discipline for writing forecast data: audit current plan against actuals, show the gap, dry run the proposed write, get explicit user approval, commit to the live database, and summarize the audit trail.

**Key objects**

- Current plan
- Actuals comparison
- Proposed adjustments
- Dry run preview
- User approval gate
- Commit confirmation
- Audit trail summary

**How to help**

- Save this forecast
- Write these numbers to the plan
- Commit this scenario
- Update the forecast

**Clarifying questions**

- Thinking the agent can commit without explicit approval
- Expecting the agent to auto-save without a dry run
- Not understanding the audit trail requirement

**Procedure**

1. **Audit the current state.** Pull the current plan data for the relevant SKUs and date range. Compare against actual sales velocity to identify the gap.
2. **Show the gap.** Present a table comparing current plan vs. actuals so the user sees exactly where the forecast diverges from reality before making changes.
3. **Capture the scenario name.** Ask the user what to name this scenario. Use their exact name and format — never rename or "improve" it.
4. **Dry run the write.** Show the user exactly what will be saved: which SKUs, which months, what values, under what scenario name. This is a preview, not a commit.
5. **Get explicit approval.** Wait for the user to confirm the dry run looks correct. Do not proceed without a clear "yes" or equivalent approval.
6. **Commit to the live database.** Write the approved data to the database under the user's scenario name.
7. **Summarize the audit trail.** Confirm exactly what was saved: scenario name, SKUs and date range affected, values written, who it's attributed to, and timestamp.

## Audit Forecast Changes

**Clarifying questions**

- Which scenario or time period do you want to review?
- Should I compare against actuals or a previous forecast version?

**How to help**

- Retrieve the committed scenario data
- Show what was saved, when, under what name, and by whom
- Compare against current actuals if requested
- Highlight any gaps between the forecast and actual performance

**Don't claim**

- Do not fabricate audit trail entries
- Do not present uncommitted data as if it were saved

**Escalate when**

- Audit trail shows unexpected modifications
- Committed data doesn't match what the user approved

## Audit Trail Review

**Procedure**

1. **Identify the scope.** Ask which scenario, time period, or SKU set the user wants to review.
2. **Retrieve committed data.** Pull the saved scenario data including scenario name, save date, and attribution.
3. **Present the record.** Show a clean table of what was committed — values, SKUs, date range, scenario name, and who approved it.
4. **Compare if requested.** If the user wants to compare against actuals or a previous version, pull that data and show a side-by-side comparison.
5. **Flag discrepancies.** If committed data doesn't align with current actuals or expectations, highlight the gaps and ask if the user wants to investigate further.

## Verified Facts

**Facts**

- Always compare proposed forecast adjustments against the current plan and actuals so the user sees the gap before approving changes.
- When saving forecast scenarios, use the exact name and format the user provides. Never rename or 'improve' user-provided labels.
- Always validate writes before persisting — show the user a dry run of what will be saved and get explicit approval before committing to the live database.
- After committing forecast data, summarize exactly what was saved, under what scenario name, and who it's attributed to — maintaining a clear audit trail.
- The forecast write workflow follows a strict sequence: audit → compare → dry run → approve → commit → summarize.
- Never commit forecast data without explicit user approval, even if the user says 'just save it' — always show the dry run first.
