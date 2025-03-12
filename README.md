# ob-team-charts
A repo for Rancher Observability &amp; Backups team's charts - a canonical dev spot just before rancher/charts.

## What O&B projects use this repo?

This covers the following O&B team charts:
- `rancher-monitoring`,
- `rancher-project-monitoring`,
- `prometheus-federator`, and
- `rancher-logging`

This is where we apply the majority of the Rancher specific changes to these charts.
None of the changes we apply at this level should be specific to a single Rancher minor version.

## How do we manage Chart version numbers?

The short version, most charts should use this syntax: `{upstream version}-rancher.{incrementing number}`.

Where each new `{upstream version}` resets the `{incrementing number}`. The result being that for any upstream version 
that we release, we will also be able to track what rancher specific changes were applied.
And that this version is universal across all Rancher minor versions that support a given `{upstream version}`.

For more specifics on why this format is used, see: [How do we manage Chart versions across this repo and `rancher/charts`?](./docs/semver-across-chart-repos.md)

## How does rebasing for Rancher Monitoring charts work with this repo?
Overall the process isn't too different, however how we manage a particular upstream version will be different.
In the new system, we will suffix each upstream version with a `-rancher.{num}` identifier.

Using that will give us a better identifier for our Rancher specific modifications that we use across Rancher minor versions.
Specifically some other ways this will benfit our team is:
- O&B team maintains a single canonical version of upstream chart changes,
- `rancher-monitoring` and `rancher-project-monitoirng` rebase can be a single PR
  - Then followed up by PRs in `rancher/prometheus-federator` and all synced to `rancher/charts` in unison. 

Please review the [Monitoring Rebase doc](./docs/monitoring-rebase.md) for more details on the process this repo allows.

