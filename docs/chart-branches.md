
# Chart Branches

This file outlines the process of how charts are released from this repository to the downstream repository.

## The `chart-branches.yaml` file

The `chart-branches.yaml` file is a structured data file that defines the relationship between the charts in this repository and the branches in the downstream repository.

This file serves two purposes:

1.  **Coordination:** It provides a single source of truth for the team to understand which chart versions are expected to be in which downstream branch. This helps to coordinate the team's work and avoid confusion.
2.  **Automation:** In the future, this file will be used by automation to create Pull Requests in the downstream repository, ensuring that the charts are always up-to-date.

## Schema

The `chart-branches.yaml` file has the following structure:

```yaml
packages:
  <package-name>:
    "<package-version>":
      - "<downstream-branch-1>"
      - "<downstream-branch-2>"
```

Where:

*   `<package-name>`: The name of the chart package, which corresponds to a directory in the `packages` directory.
*   `<package-version>`: The version of the package. This corresponds to a sub-directory within the package's directory.
*   `<downstream-branch>`: The name of the branch in the downstream repository that should receive updates from this package version.

### How it is used

The file is a mapping that indicates which downstream branches should receive chart updates for a specific version of a chart package.

For example, if `chart-branches.yaml` contains:

```yaml
packages:
  rancher-logging:
    "4.10":
      - "release/v2.6"
      - "release/v2.7"
```

This means that any updates to the `4.10` version of the `rancher-logging` package should be propagated to the `release/v2.6` and `release/v2.7` branches in the downstream repository.


## Future consideration

This file could be used by future automation tooling that would help create PRs.
These tools would have to resolve Chart versions from Packages via `package.yaml`.