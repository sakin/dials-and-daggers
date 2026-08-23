# Sync Plan Template

A sync plan is written to `.scratch/rails-starter-sync-<date>.md`.

## Required sections

### 1. Header

- Date
- Upstream commit and branch
- Downstream commit and branch
- Merge base

### 2. Summary

- Total commits from upstream since merge base
- Total files changed
- Insertions / deletions

### 3. Categorized file list

#### Safe to merge

List files here with a one-line description of the change.

#### Merge carefully

List `AGENTS.md`, `CONTEXT.md`, and any other file that needs a three-way merge with human judgment.

#### Review first

List files that are project-specific or carry significant behavioral risk. Include a sentence explaining the risk.

#### Skip

List `docs/adr/*` and any other file the plan intentionally excludes.

### 4. Risks and manual follow-up

Call out behavioral risks such as:

- Default form builder now applies to every `form_with`.
- Checkbox/toggle no longer emits Rails' hidden unchecked twin.
- `ApplicationViewComponent` now reserves `attrs`, `class`, `id`, and `data` root arguments.
- Project-name residue remains in `deploy.yml`, `rename_to.rb`, `Dockerfile`, etc.

### 5. Review checklist

- [ ] Read the plan.
- [ ] Confirm the "review first" files are acceptable.
- [ ] Resolve any merge conflicts.
- [ ] Run `bin/ci`.
- [ ] Rename remaining `rails_starter`/`RailsStarter` references.
- [ ] Commit the merge.
