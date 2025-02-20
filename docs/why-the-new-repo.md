# FAQs about the new repo

## What is this repository for compared to `rancher/charts`?
This repo is the O&B teams primary source of truth for the Charts we maintain.
Primarily it is focused on the charts we pull from 3rd party sources.
But can also be used for internal produced charts from O&B team if it makes sense.

## What issues does this repo solve compared to the existing `rancher/charts` repo and process?
This repository solves a unique set of problems that the O&B team have while maintaining our charts Monitoring and Logging charts.
At a high level, these problems are:
- Issues with Regressions caused by current rebase workflows,
- Missed dependency bumps during current rebase causing usage of more image tags than necessary,
  - In other words, we do a rebase and expect to remove CVEs, but they remain because some charts use old tags.
- Varying patches based on Upstream Chart Version for Rancher Branch on `rancher/charts`

## What factors make the current (or old) chart process for O&B complex?
- We have 3 distinct types of changes that are made to `rancher/charts` by our team:
  - Chart Rebase Work,
  - Chart Image Bumps and Security Patches,
  - Rancher Specific Changes
- Of those sets of change types, each may (or may not) have subsets of:
- Specific Image Tags Needed to Match Upstream chart targets,
- Rancher Version dependant changes

These variables cause our team to repeat a lot of work, but just subtly different.
However, the cognitive load of comparing similar, but different, patches for the same upstream charts on different branches can be taxing.

## Why only `main` branch in this repo?
The principal function of this repo is to reduce redundant work from processes used by the O&B team today.
The current process to maintain Monitoring and Logging -essentially- involve us maintaining the same upstream version in multiple Rancher Branches.

Instead, if we apply most of our Rancher specific changes in a Rancher version agnostic manner we can maintain important changes once.
Then when the chart lands in a Minor version specific `rancher/charts` branch it can have Rancher minor specific patching applied.

### What about `rancher/charts` auto-bumps?
This workflow change isn't something considered as part of this repo's creation.

However, the requirements of that new feature of `rancher/charts` isn't incompatible with our new repo.
It will complicate it if we must adopt it, yet it's possible to do via using `main` as our source of truth, and pulling from it to produce Rancher Minor specific charts on release branches here.
Likely we should start the new release branches from a single common orphaned branch

## Where does the chart for Helm Project Operator and Prometheus Federator live now?
The chart for `helm-project-operator` that used to be a subchcart of Prometheus Federator was removed.
It was primarily used for development testing and complicated the development of the chart for Prometheus Federator.
So by removing it we simplified the Prometheus Federator chart, which now lives in the Prometheus Federator repo.