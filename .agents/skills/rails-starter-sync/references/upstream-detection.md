# Upstream Detection

The template is `sakin/rails-starter`. The sync skill looks for it in this order:

1. **Git remotes** in the current project. Acceptable names or URLs:
   - `rails-starter` (or any remote whose URL contains `rails-starter`)
   - `rails_starter`
   - `sakin/rails-starter` or `sakin/rails_starter`

2. **Sibling directory** on the filesystem:
   - `<project-root>/../rails_starter`
   - `<project-root>/../rails-starter`

3. **Fail** if neither is found. Report the missing upstream and stop.

When a remote is found, fetch it before comparing. When a local clone is found, verify it is on `main` and up to date with `origin/main` before comparing.

The comparison is always against upstream `main` unless the human asks for a different branch.
