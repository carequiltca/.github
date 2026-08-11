# carequiltca/.github

Organization-wide GitHub defaults for CareQuilt.

## Pull request template

`pull_request_template.md` is used as the default PR template for repositories that do not define their own.

## Risk label required check

Pull requests must include exactly one of these risk labels:

- `Risk - None`
- `Risk - Minimal`
- `Risk - Moderate`
- `Risk - Critical`

### Workflows

| File | Purpose |
| --- | --- |
| `.github/workflows/risk-label-check.yml` | Reusable workflow with the label validation logic |
| `.github/workflows/risk-label-required.yml` | Org ruleset entry point; runs on PR open/update and merge queue |
| `.github/workflow-templates/risk-label-on-label.yml` | Optional per-repo workflow; re-runs the check when labels change |

### 1. Enable org-wide enforcement (required)

1. In the organization, open **Settings → Repository → Rulesets**.
2. Create or edit a ruleset that targets the repositories and branches you want to protect.
3. Add **Require workflows to pass before merging**.
4. Select `carequiltca/.github/.github/workflows/risk-label-required.yml@main`.
5. Save the ruleset.

If the `.github` repository is private or internal, allow other repositories in the organization to use its workflows and reusable workflows.

Organization rulesets run required workflows on `pull_request` open, synchronize, and reopen events. They do not automatically re-run when a label is added.

### 2. Re-run on label changes (recommended)

To automatically re-evaluate the check after a risk label is added or removed, add the workflow template to each repository:

1. In a repository, go to **Actions → New workflow**.
2. Choose **Risk label check (on label change)** from the organization workflow templates (or copy `.github/workflow-templates/risk-label-on-label.yml` into `.github/workflows/`).
3. Commit the workflow to the default branch.

This template uses the same workflow and job names as the org ruleset workflow, so a passing run updates the same required status check and unblocks merge without a new commit.

If a repository does not include the on-label workflow, add the risk label and then use **Re-run failed jobs** on the failed **Risk Label Required** check.
