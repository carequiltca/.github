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
| `.github/workflows/risk-label-required.yml` | Org ruleset entry point; runs on PR open/update, label changes, and merge queue |
| `.github/workflow-templates/risk-label-on-label.yml` | Legacy per-repo fallback for older org workflow versions |

### 1. Enable org-wide enforcement (required)

1. In the organization, open **Settings → Repository → Rulesets**.
2. Create or edit a ruleset that targets the repositories and branches you want to protect.
3. Add **Require workflows to pass before merging**.
4. Select `carequiltca/.github/.github/workflows/risk-label-required.yml@main`.
5. Save the ruleset.

If the `.github` repository is private or internal, allow other repositories in the organization to use its workflows and reusable workflows.

Organization rulesets only *require* the check on `pull_request` open, synchronize, and reopen events. The org workflow also listens for `labeled` and `unlabeled` events so the check re-runs automatically when a risk label is added or removed—no new commit needed.

After merging workflow changes to `main`, add or remove a label on an existing PR to confirm the **Risk Label Required** check re-runs.

### 2. Re-run on label changes (legacy fallback)

Repositories that pinned an older version of the org workflow (without `labeled` / `unlabeled` triggers) can still add the workflow template locally:

1. In a repository, go to **Actions → New workflow**.
2. Choose **Risk label check (on label change)** from the organization workflow templates (or copy `.github/workflow-templates/risk-label-on-label.yml` into `.github/workflows/`).
3. Commit the workflow to the default branch.

This template uses the same workflow and job names as the org ruleset workflow, so a passing run updates the same required status check.

If the check still does not re-run after a label change, use **Re-run failed jobs** on the failed **Risk Label Required** check.
