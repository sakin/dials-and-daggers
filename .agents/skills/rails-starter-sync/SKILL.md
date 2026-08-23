---
name: rails-starter-sync
description: Sync a project bootstrapped from sakin/rails-starter with the latest template changes. Detects the upstream template, generates a categorized sync plan, applies the merge, and runs the test suite.
---

# Rails Starter Sync

Use this skill when a project was created from `sakin/rails-starter` and you want to pull recent template updates into it.

The skill is **opinionated** to protect project-specific work:

- It always **generates a plan first** and waits for confirmation before applying anything.
- It **merges** upstream `main` into the current branch.
- It **merges `AGENTS.md` and `CONTEXT.md` carefully** so project-specific terms are not overwritten.
- It **skips template ADRs** in `docs/adr/` because downstream projects write their own.
- It **flags project-name residue** (`RailsStarter`, `rails_starter`, deploy config, rename tool) for manual renaming.
- It **runs the test suite** after the merge.

## When to use this skill

- The upstream template has new commits you want in your project.
- You want a reproducible, reviewable sync process.
- You are unsure which upstream files are safe to merge and which need human review.

## Workflow

### 1. Locate the upstream template

1. Check git remotes in the current project. Look for `rails-starter`, `rails_starter`, `sakin/rails-starter`, or `sakin/rails_starter`.
2. If a remote exists, fetch it.
3. If no remote exists, check for a local clone at the sibling path of the current project: `../rails_starter` (or `../rails-starter`).
4. If neither exists, report the problem and stop. The sync cannot proceed without the template.

### 2. Find the merge base

1. Identify the current branch/commit of the downstream project.
2. Identify the tip of upstream `main`.
3. Compute the merge base between the two.
4. Report the delta: commits ahead/behind, files changed, and an overall stat.

### 3. Categorize changes

Group the files into four buckets:

| Category | Typical files | Action |
|---|---|---|
| **Safe to merge** | `Gemfile*`, `config/` (non-application.rb), `lib/`, `bin/`, `spec/`, `app/views/components/`, `app/frontend/`, `app/views/components/` (new components), `package.json`, `vite.config.mts`, `bun.lock` | Merge automatically |
| **Merge carefully** | `AGENTS.md`, `CONTEXT.md` | Three-way merge, preserve downstream additions, then reconcile conflicts manually |
| **Review first** | `app/views/controllers/pages/home.html.erb`, auth views (`sessions/new`, `registrations/new`, `passwords/*`), `config/application.rb` (form-builder default), `config/deploy.yml`, `lib/rename_to.rb`, `Dockerfile` | Decide case-by-case; the skill flags them in the plan |
| **Skip** | `docs/adr/*` | Do not overwrite downstream ADRs |

Notes on specific files:

- `config/application.rb` is only **safe** if the downstream project is still using the `RailsStarter` module and has no form-builder customizations. If the project has renamed, skip it and apply the relevant parts manually.
- `app/views/components/form_field/` and `app/views/components/submit_button/` were removed in upstream in favor of `Forms::Field` and `Forms::Submit`. If they exist downstream, flag them for removal.
- `app/views/components/forms/*` is new upstream. Merge it.

### 4. Generate the sync plan

Write a markdown file at `.scratch/rails-starter-sync-<date>.md` containing:

1. Upstream location and commit.
2. Downstream commit and branch.
3. Merge base.
4. Stat summary.
5. The categorized file list.
6. Specific risks and manual follow-up steps.
7. A checklist for the human reviewer.

Use the format in [references/sync-plan-template.md](references/sync-plan-template.md).

### 5. Review with the human

Present the plan. Ask for confirmation. If the human wants to change a category, update the plan and ask again. Do not apply the merge until the human confirms.

### 6. Apply the merge

1. Run `git merge --no-commit --no-ff upstream/main` (or the local equivalent).
2. If there are conflicts, follow the `resolving-merge-conflicts` skill.
3. For files marked **skip**, reset them to the downstream version:
   - `git checkout --ours docs/adr/`
4. For files marked **merge carefully**, resolve them manually, preserving downstream content.
5. For **review first** files, leave them in the working tree unmodified so the human can inspect them.
6. Stage the remaining safe files.
7. Do **not** commit. The merge should be left in the working tree for review.

### 7. Run the test suite

1. Install dependencies: `bundle install` and `bun install` if needed.
2. Run the CI script: `bin/ci`.
3. If system specs fail, check whether the container/CI environment is the cause. See [references/post-merge-checklist.md](references/post-merge-checklist.md).
4. Report failures and stop for fixes.

### 8. Final report

Summarize:

- What was merged.
- What was skipped.
- What files still need manual review.
- What project-name residue remains.
- Test results.
- Reminder to commit when ready.

## Reference documents

- [upstream-detection.md](references/upstream-detection.md) — how to find the template.
- [sync-plan-template.md](references/sync-plan-template.md) — plan format.
- [exclusions.md](references/exclusions.md) — default exclusion list and reasoning.
- [post-merge-checklist.md](references/post-merge-checklist.md) — what to verify after the merge.
