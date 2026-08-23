# Default Exclusions

These files are excluded by default from an automatic merge:

| Path | Reason |
|---|---|
| `docs/adr/*` | Downstream projects write their own architecture decisions. Overwriting them would destroy project-specific rationale. |
| `app/views/controllers/pages/home.html.erb` | Usually contains template marketing copy. A real project should write its own home page. |
| `config/deploy.yml` | Contains service/image names and host config that are tied to the project name. |
| `lib/rename_to.rb` | The rename tool hard-codes the old template name for the source project. It should be regenerated after a project rename. |
| `Dockerfile` | Comments may mention the template name; rarely contains code changes worth merging. |

Files that are **merged carefully** rather than excluded:

| Path | Reason |
|---|---|
| `AGENTS.md` | May contain template-wide instructions that should propagate, but project-specific additions must be preserved. |
| `CONTEXT.md` | Template glossary terms should be merged, but project-specific terms must be preserved. |
| `config/application.rb` | The module name and the default form builder may be project-specific. |

## Optional overrides

The human can request that any excluded file be merged anyway. In that case, note the override in the sync plan and merge it manually.
