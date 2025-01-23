# ob-team-charts
A repo for Rancher Observability &amp; Backups team's charts - a canonical dev spot just before rancher/charts.

## How does re-basing of Rancher Monitoring Charts work with this repo involved?
For now, it doesn't yet - but when we're fully in flight this will be updated to reflect that.
Today this repo is only used to benefit Prometheus Federator, but I hope to incorporate with our next Monitoring Rebase.
Then after that we'll start to onboard logging charts as well.

---

Additional details on the necessity and functionality of this repo are detailed here:

## What is this repository for - why not just `rancher/charts`?
This repository solves a unique set of problems that the O&B team has while maintaining our charts.
At a high level, these problems are:
- Repeated issues with Regressions caused by current rebase workflows,
- Missed dependency bumps during current rebase causing usage of more image tags than necessary,
  - In other words, we do a rebase and expect to remove CVEs, but they remain because some charts use old tags.
- Varying patches based on Upstream Chart Version for Rancher Branch on `rancher/charts`

## Why does the O&B team have these challenges?
We maintain a number of charts - and sub-chart dependencies - that are coming from external upstream sources.
Each of these charts have unique upstream versions we target periodically in rebase cycles.
That triggers updates to be necessary for all Rancher branches getting the rebase; and this is a common point that risks regressions.

## What factors make the current chart process for O&B complex?
- We have 3 distinct types of changes that are made to `rancher/charts` by our team:
  - Chart Rebase Work,
  - Chart Image Bumps and Security Patches,
  - Rancher Specific Changes
- Of those sets of change types, each may (or may not) have subsets of:
 - Specific Image Tags Needed to Match Upstream chart targets,
 - Rancher Version dependant changes

These variables cause our team to repeat a lot of work, but just subtly different.
However, the cognitive load of comparing similar, but different, patches for the same upstream charts on different branches can be taxing.