# How to test charts straight from ob-team-charts

The ob-team-charts repository is used by the ORBS team to handle all of its custom Helm charts, but Rancher does not pull them directly from this repo. Rancher only pulls from the `rancher/charts` repository by default, so some extra steps are needed if you want to run and test charts from `rancher/ob-team-charts` or from a fork.

## Creating a ClusterRepo

First step is to create a custom ClusterRepo resource as shown in the example:

```yaml
## ClusterRepo pulling from the main branch in the original ob-team-charts repo
apiVersion: catalog.cattle.io/v1
kind: ClusterRepo
metadata:
  name: ob-team-charts
spec:
  gitBranch: main
  gitRepo: https://github.com/rancher/ob-team-charts

```

This ClusterRepo definition targets the main branch under the original ob-team-charts repository. If instead you want to pull straight from a fork, simply specify the correct Github organization and repository name in the link under *gitRepo* and the target branch under *gitBranch*. For instance:

```yaml
## ClusterRepo pulling from the rancher-monitoring-69.8.2 branch in the ob-team-charts fork by 'jbiers' Github user
apiVersion: catalog.cattle.io/v1
kind: ClusterRepo
metadata:
  name: ob-team-charts-jbiers
spec:
  gitBranch: rancher-monitoring-69.8.2
  gitRepo: https://github.com/jbiers/ob-team-charts

```
Having your ClusterRepo definition, save it as a YAML file and run `kubectl apply -f <my-clusterrepo-file>` or import the file via Rancher UI.

## Installing charts from ob-team-charts

With the ClusterRepo created Rancher should now be pulling charts from the desired repo. In the Rancher UI, go to **Apps > Charts** and you will see a dropdown menu indicating all sources from which Rancher is pulling charts. Uncheck all others and leave only the one you just created, its name will reflect the metadata.name field.

You also need to allow Rancher to show pre-release Charts. This can be done also by Rancher UI via **Preferences > Helm Charts > Include Prerelease Versions**.

Now, all charts defined in the target repository can be installed directly from the Apps section in Rancher UI as usual.
