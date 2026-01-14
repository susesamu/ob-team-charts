# ob-team-charts
A repo for Rancher Observability &amp; Backups team's charts - a canonical dev spot just before rancher/charts.

## What ORBS projects use this repo?

This covers the following ORBS team charts:
- `rancher-monitoring`,
- `rancher-project-monitoring`, and
- `rancher-logging`

This is where we apply the majority of the Rancher specific changes to these charts.
None of the changes we apply at this level should be specific to a single Rancher minor version.

## How do we manage Chart version numbers?

The short version, most charts should use this syntax: `{upstream version}-rancher.{incrementing number}`.

Where each new `{upstream version}` resets the `{incrementing number}`. The result being that for any upstream version 
that we release, we will also be able to track what rancher specific changes were applied.
And that this version is universal across all Rancher minor versions that support a given `{upstream version}`.

For more specifics on why this format is used, see: [How do we manage Chart versions across this repo and `rancher/charts`?](./docs/semver-across-chart-repos.md)

## How are PRs merged into this repo?
Due to how we manage the version numbers here, read the section above for detail, we must ensure that we only merge PRs after charts are valdiated. This means that PRs must have both: approving review from ORBS team, and a validation comment on the PR or issue from ORBS QA team.

This way every version published to main is a "safe version" - meaning while they may still have bugs, none should have critical flaws. This gives us greater confidence to ship multiple changes to `rancher/charts` at one time (when we release) - as all changes were tested at least once before that.

For more on what details to provide QA for testing, see: [How to test charts straight from ob-team-charts](./docs/testing-from-ob-team-charts.md).

## How does rebasing for Rancher Monitoring charts work with this repo?
Overall the process isn't too different, however how we manage a particular upstream version will be different.
In the new system, we will suffix each upstream version with a `-rancher.{num}` identifier.

Using that will give us a better identifier for our Rancher specific modifications that we use across Rancher minor versions.
Specifically some other ways this will benfit our team is:
- ORBS team maintains a single canonical version of upstream chart changes,
- `rancher-monitoring` and `rancher-project-monitoirng` rebase can be a single PR
  - Then followed up by PRs in `rancher/prometheus-federator` and all synced to `rancher/charts` in unison. 

Please review the [Monitoring Rebase doc](./docs/monitoring-rebase.md) for more details on the process this repo allows.

## Repo name ORBS vs O&B (ob)
The team has been renamed from O&B to ORBS. The repo name will remain the same.