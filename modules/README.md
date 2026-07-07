# Modules

Optional add-ons that plug into the lifecycle without weighing down projects
that don't need them.

## How a module works

A module is two things:

1. **An asset folder** `modules/<name>/` with a `module.yaml` manifest
   (name, version, description, provides, skills) plus its guides and
   templates.
2. **Prefix-named skills** in `.claude/skills/` (`bk-*`, `cv-*`, `nl-*`)
   whose first step checks the module is enabled and stops politely if not.

Enablement is a list in `project/project.yaml` (set during `/project-init`,
editable any time):

```yaml
modules:
  - nl-deliverables
```

`init_project.py --list-modules` discovers modules by scanning
`modules/*/module.yaml`. No symlinks, no install step — the cost of a
disabled module is a few skill-description lines.

## Shipped modules

| Module | What it adds |
|---|---|
| [brand-kit-studio](brand-kit-studio/) | Interview-driven creation of brand kits (`design/brand-kits/<kit>/`) that compile into themes and hard-rule tokens |
| [custom-visual-studio](custom-visual-studio/) | Scaffold and develop a `pbiviz` TypeScript custom visual when native visuals genuinely can't serve an intent |
| [nl-deliverables](nl-deliverables/) | Dutch client deliverables: UAT/testplan and definitie-pagina content generated from the contracts |

## Writing a new module

1. Create `modules/<name>/module.yaml` (keep it within the miniyaml subset —
   see `lifecycle/status.schema.yaml`'s header) + README + assets.
2. Add its skills under `.claude/skills/<prefix>-<name>/` with the
   enablement check as step 1.
3. Keep module knowledge in the module: lifecycle skills may *mention* a
   module (as s7 mentions nl-uat) but must degrade gracefully without it.

## Future module ideas

- **backend-stories** — reverse-engineer a PBIP's DAX/M into backend user
  stories, for teams moving report logic into ETL/SQL.
- **fabric-deploy** — deployment-pipeline / Fabric CLI adapter with its own
  safety hooks (deploys stay user-run).
- **data-profiler** — source data-quality profiling that feeds stage 1's
  data landscape with evidence instead of hearsay.
