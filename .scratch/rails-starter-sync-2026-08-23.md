# Rails Starter Sync Plan — dials-and-daggers

**Date:** 2026-08-23
**Downstream:** `dials-and-daggers` `main` @ `945cd21`
**Upstream:** `sakin/rails-starter` `main` @ `a681ac4`
**Upstream branch:** `main`
**Merge base:** No shared git history (the downstream repo was bootstrapped from a snapshot). The equivalent upstream baseline is `92edffc` (pre-forms), and the downstream `945cd21` matches that snapshot plus the same CI hardening that is now in upstream `main`.
**Delta:** 103 files changed, +1733 / -93 lines. The delta is entirely the merged `feature/forms` branch.

## Summary

The upstream template has shipped the `feature/forms` work. The only changes since the downstream snapshot are the new form-component family, the `ApplicationFormBuilder`, a `Modal::Component`, `dry-types` coercion, `tailwind_merge`, updated skills, and the associated tests/previews.

## Safe to merge

| File | Change |
|---|---|
| `Gemfile` | Adds `dry-types` and `tailwind_merge` |
| `Gemfile.lock` | Resolved versions for new gems |
| `config/initializers/dry_types.rb` | New initializer requiring `dry/types` |
| `app/frontend/entrypoints/application.js` | Globs co-located component controllers; converts controller filenames to hyphenated names |
| `app/frontend/controllers/modal_open_controller.js` | New flat trigger controller for modals |
| `app/views/components/application_view_component.rb` | Common root args, `element_attrs`, `StyleVariants`, `tailwind_merge` |
| `app/views/components/forms/*` | New form-component family |
| `app/views/components/modal/*` | New modal component |
| `app/views/components/banner/*/preview.rb` | Uses `render` instead of `render_component` |
| `app/views/components/flash/preview.rb` | Uses `render` |
| `app/views/components/layout/preview.rb` | Uses `render` |
| `app/views/components/mailer/preview.rb` | Uses `render` |
| `app/views/components/nav/preview.rb` | Uses `render` |
| `lib/application_form_builder.rb` | New `ActionView::Helpers::FormBuilder` subclass |
| `lib/generators/view_component/*` | Generator emits typed options and uses `render` |
| `spec/forms/application_form_builder_spec.rb` | New builder tests |
| `spec/views/components/application_view_component_spec.rb` | New base-class tests |
| `spec/views/components/forms/*` | New component tests |
| `spec/views/components/modal_component_spec.rb` | New modal test |
| `spec/previews/*` | New Lookbook preview tests |
| `spec/system/forms/floating_label_spec.rb` | New system spec |
| `spec/system/modal_spec.rb` | New system spec |
| `spec/generators/view_component_generator_spec.rb` | Updated for typed options |
| `.agents/skills/local-environment/*` | New skill |
| `.agents/skills/rails-startup-stack/*` | Updated references, cross-link to local-environment |

## Merge carefully

| File | Reason |
|---|---|
| `AGENTS.md` | Upstream adds an ASD-STE100 / CONTEXT.md reading instruction. Project-specific instructions already exist and must be preserved. |
| `CONTEXT.md` | Upstream adds new glossary terms (preview/scenario, forms family, component arguments, modal) and updates `primitive`. Project-specific terms and the "Rails Starter" framing must be preserved/reconciled. |

## Review first

| File | Risk |
|---|---|
| `config/application.rb` | Sets `config.action_view.default_form_builder = "ApplicationFormBuilder"`. This changes every `form_with` in the app. |
| `app/views/controllers/pages/home.html.erb` | Now contains a modal demo / "Rails Starter" marketing copy. |
| `app/views/controllers/sessions/new.html.erb` | Uses new forms; may conflict with custom auth work later. |
| `app/views/controllers/registrations/new.html.erb` | Uses new forms; contains template demo copy. |
| `app/views/controllers/passwords/new.html.erb` | Uses new forms; contains template demo copy. |
| `app/views/controllers/passwords/edit.html.erb` | Uses new forms; contains template demo copy. |

## Skip

| Path | Reason |
|---|---|
| `docs/adr/0003-forms-components-and-builder.md` | Downstream ADRs should be written by this project. |

## Removed upstream files

These files existed in the downstream snapshot but were replaced by the `Forms::*` family:

- `app/views/components/form_field/*` → replaced by `app/views/components/forms/field/*`
- `app/views/components/submit_button/*` → replaced by `app/views/components/forms/submit/*`
- `spec/views/components/form_field_component_spec.rb` → replaced by `spec/views/components/forms/field_component_spec.rb`
- `spec/views/components/submit_button_component_spec.rb` → replaced by `spec/views/components/forms/submit_component_spec.rb`

## Risks and manual follow-up

1. **Default form builder** now applies to every `form_with`.
2. **Checkbox/toggle** no longer emits Rails' hidden unchecked twin. Existing boolean fields may need review.
3. **`ApplicationViewComponent`** reserves `attrs`, `class`, `id`, and `data` root arguments.
4. **Project-name residue** remains in `config/deploy.yml`, `lib/rename_to.rb`, `Dockerfile`, `config/application.rb` (`module RailsStarter`), and `CONTEXT.md` (`Rails Starter`).

## Review checklist

- [ ] Read the plan.
- [ ] Confirm the "review first" files are acceptable.
- [ ] Resolve `AGENTS.md` and `CONTEXT.md` carefully.
- [ ] Restore `docs/adr/` from the downstream version.
- [ ] Run `bin/ci`.
- [ ] Rename remaining `rails_starter` / `RailsStarter` references.
- [ ] Commit the merge.

## Plan approval

Approved: apply the merge, bring in the forms stack, auth views, and home page; skip ADRs; merge `AGENTS.md`/`CONTEXT.md` carefully.
