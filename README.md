# Logic Apps Standard custom-code .NET 10 migration

A migration skill for Azure Logic Apps Standard local-function libraries that
currently target .NET 8. It guides configuration changes, publication checks, and
optional coordinated debugging in VS Code without rewriting your workflows or C#.

This is a community-maintained skill, not an official Microsoft support statement.
It is not yet part of Awesome Copilot.

## Install with GitHub CLI

Use GitHub CLI 2.90.0 or later. The `gh skill` commands are currently preview.
Inspect the content before installation:

```powershell
gh skill preview wsilveiranz/custom-code-net10 logicapps-custom-code-net10-migration@v0.1.0
gh skill install wsilveiranz/custom-code-net10 logicapps-custom-code-net10-migration@v0.1.0 --agent github-copilot --scope user
```

`--scope user` makes the skill available to Copilot across projects. To install only
in a target repository, run the command there with `--scope project` instead.
Installing the skill does not run the migration.

Alternatively, download the complete skill ZIP from the
[v0.1.0 release](https://github.com/wsilveiranz/custom-code-net10/releases/tag/v0.1.0)
and copy its skill folder into `%USERPROFILE%\.copilot\skills` or your target
repository's `.github\skills` directory. Do not copy only `SKILL.md`.

## Prerequisites

- Azure Logic Apps (Standard) VS Code extension with .NET 10 custom-code support.
- Logic Apps Standard extension bundle 1.194.31.
- VS Code-selected .NET 10 SDK/runtime, alongside the existing .NET 8 host runtime.
- Python 3.11+ for the standard-library configuration helper.

The skill is for Logic Apps Standard custom-code libraries, not generic .NET apps,
standalone Azure Functions, or Consumption workflows.

## Use

Back up your target workspace first, including local configuration. Keep backups
containing credentials private and outside source control.

Ask your agent:

> Migrate this Logic Apps Standard .NET 8 custom-code workspace to .NET 10 using
> the logicapps-custom-code-net10-migration skill. Preserve my workflows and function
> identities, review the dry-run plan first, and include coordinated VS Code debugging.

The helper defaults to a read-only dry run. It does not install SDKs, restore
packages, build code, start hosts, create persistent backups, or change Azure.
Agent-led build and runtime checks are separate steps requiring appropriate
authorization. Unsupported or ambiguous configurations require manual review.

For Azure execution, application settings must include both
`LOGIC_APPS_CUSTOMCODE_DOTNETVERSION=net10.0` and `FUNCTIONS_INPROC_NET8_ENABLED=1`.
The Logic Apps VS Code deployment copies these settings. Pipeline deployments must
explicitly include both. Verify the deployed runtime separately.

See the [package README](skills/logicapps-custom-code-net10-migration/README.md)
for helper commands, optional flags, and limitations.

## Validate the package

From this repository's root:

```powershell
python -m unittest discover -s ".\skills\logicapps-custom-code-net10-migration\tests" -v
gh skill publish --dry-run
```

The tests use synthetic configuration, not live workflows or Azure resources.
Successful configuration edits or builds do not establish successful execution.

Report migration issues through this repository's issues. Include sanitized
configuration and diagnostics, never credentials or private payloads.

## License

[MIT](LICENSE).
