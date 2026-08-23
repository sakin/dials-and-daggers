# Post-Merge Checklist

After the upstream merge is applied and before committing, run through this checklist.

## Automated checks

1. `bundle install` — install any new gems.
2. `bun install` — install any new frontend dependencies.
3. `bin/ci` — run the full CI suite.
4. If `bin/ci` is unavailable, run `bundle exec rspec` and `bin/rubocop`.

## Common failure modes

### System specs fail

- Check that the browser/driver is available in the environment.
- Check that `SELENIUM_HOST`, `REMOTE_SELENIUM`, or similar CI env vars are set correctly.
- If the failure is environment-specific, note it and skip system specs only after confirming the code is correct.

### Form-related specs fail

- The default form builder now applies to every `form_with`.
- Verify that `config/application.rb` has `config.action_view.default_form_builder = "ApplicationFormBuilder"`.
- Check that any existing `check_box` or `toggle` usages are compatible with the new components.

### Generator specs fail

- The `view_component` generator now emits typed options.
- Update generator specs to match the new output.

### Lookbook preview specs fail

- New components require a `preview.rb` and a matching `preview.html.erb`.
- Ensure all previews render without errors.

## Manual review

1. Inspect the home page (`app/views/controllers/pages/home.html.erb`).
2. Inspect the authentication views.
3. Check for remaining `RailsStarter` or `rails_starter` references.
4. Verify `AGENTS.md` and `CONTEXT.md` still contain project-specific content.
5. Review the commit diff before committing.

## Final commit

When the suite is green and the manual review is done, stage everything and commit:

```
git add -A
git commit -m "Merge rails-starter updates into <branch>"
```

Use a merge commit if the history should preserve the upstream lineage.
