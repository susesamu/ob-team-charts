# Docs for Dev Scripts

> [!NOTE]
> Currently all the dev-scripts are wrappers for a golang CLI tool. New dev tools can be added as either bash scripts or part of the golang CLI.

## Verify Chart Images

The verify-chart-images script checks whether the Docker images referenced in a Helm chart exist in Docker Hub.
It can process images from either:

- A Rancher Monitoring chart version (provided as an argument, must be in `charts/rancher-monitoring`).
- Standard input, such as the output of a Helm install dry-run.

### Usage

You can run the script in two ways:

1. **By specifying a Rancher Monitoring chart version:**
   ```bash
   ./dev-scripts/verify-chart-images <chart_version>
   ```
   Example:
   ```bash
   ./dev-scripts/verify-chart-images 66.7.1-rancher.1
   ```

> [!NOTE]
> In this mode you should setup a `debug.yaml` (values.yaml) file generated from Rancher.
> In the future we may provide a default file that does the equivalent in a generic way.

2. **By piping Helm debug output to the script:**
   ```bash
   helm install --dry-run --debug rancher-monitoring ./charts/rancher-monitoring/<chart_version> -n cattle-monitoring-system | ./dev-scripts/verify-chart-images
   ```
   Example:
   ```bash
   helm install --dry-run --debug rancher-monitoring ./charts/rancher-monitoring/57.0.3-rancher.1 -n cattle-monitoring-system | ./dev-scripts/verify-chart-images
   ```

3. **With a values file for additional customization:**
   ```bash
   helm install --dry-run --debug rancher-monitoring ./charts/rancher-monitoring/<chart_version> -f ./debug.yaml -n cattle-monitoring-system | ./dev-scripts/verify-chart-images
   ```
   Example:
   ```bash
   helm install --dry-run --debug rancher-monitoring ./charts/rancher-monitoring/57.0.3-rancher.1 -f ./debug.yaml -n cattle-monitoring-system | ./dev-scripts/verify-chart-images
   ```

### Output Example
Primary output will be a table similar to:
```bash
+----+-------------------------------------------------------------------------+--------+
|  # | IMAGE                                                                   | STATUS |
+----+-------------------------------------------------------------------------+--------+
|  1 | rancher/mirrored-prometheus-alertmanager:v0.27.0                        | ✅     |
|  2 | rancher/mirrored-library-nginx:1.24.0-alpine                            | ✅     |
|  3 | rancher/mirrored-thanos-thanos:v0.34.1                                  | ✅     |
|  4 | rancher/mirrored-kube-state-metrics-kube-state-metrics:v2.10.1          | ✅     |
|  5 | rancher/mirrored-prometheus-node-exporter:v1.7.0                        | ✅     |
|  6 | rancher/mirrored-prometheus-operator-prometheus-operator:v0.72.0        | ✅     |
|  7 | rancher/shell:v0.2.1                                                    | ✅     |
|  8 | rancher/mirrored-prometheus-windows-exporter:0.25.1                     | ✅     |
|  9 | rancher/mirrored-prometheus-adapter-prometheus-adapter:v0.12.0          | ✅     |
| 10 | rancher/mirrored-prometheus-operator-prometheus-config-reloader:v0.72.0 | ✅     |
| 11 | rancher/mirrored-kiwigrid-k8s-sidecar:1.26.1                            | ✅     |
| 12 | rancher/kubectl:v1.20.2                                                 | ✅     |
| 13 | rancher/mirrored-prometheus-prometheus:v2.50.1                          | ✅     |
| 14 | rancher/mirrored-grafana-grafana:10.4.9                                 | ✅     |
| 15 | rancher/mirrored-ingress-nginx-kube-webhook-certgen:v1.4.3              | ✅     |
+----+-------------------------------------------------------------------------+--------+
```

Above this table, will be additional information about the checks done. Take note of any section like:

```bash
👨‍🔧 These need manual checks:
● image: {{ template system_default_registry . }}{{ .Values.proxy.image.repository }}:{{ .Values.proxy.image.tag }}
● image: {{ template system_default_registry . }}{{ .Values.prometheus.prometheusSpec.proxy.image.repository }}:{{ .Values.prometheus.prometheusSpec.proxy.image.tag }}
```

### Dependencies

Ensure the following dependencies are installed:
- bash
- helm (for processing chart versions)

## Get Rebase Target Info

When starting a rebase process for monitoring, we often spend some time collecting information and generally taking notes about chart and image versions.
Or we have to discover those versions as we do the process and encounter new versions for charts and images.

This tool seeks to help speed up that process by collecting relevant chart information based on upstream target versions.

## Usage

You can run the script with:
   ```bash
   ./dev-scripts/get-rebase-info <upstream chart version>
   ```
Example:
   ```bash
   ./dev-scripts/get-rebase-info 66.7.1
   ```

### Output Example

After it runs, assuming you don't see errors, you can find the `rebase.yaml` file with rebase details.
It will be in the project `ob-team-charts` project root (where ever you cloned the git repo).

An overview of how to use each section of the rebase info file:
- `target_version` - the upstream chart version
- `found_chart` - the specific details about the found chart matching the target version
- `chart_dependencies` - the list of sub-chart dependencies for the found chart (directly from Chart.yaml)
- `dependency_chart_versions` - the list of specific sub-chart dependencies to use for this rebase (selects the highest valid chart at time of running)
  - Each of these indicate what version of the sub-chart will be used and the git hash to use for it.
  - Make sure that the `packages/rancher-monitoring/{version}/{sub-chart}/package.yaml` uses the correct hash.
- `charts_images_lists` - this lists all found images grouped by the chart it is needed for.
  - This should be used to review and update the list of images in `image-mirror`.
  - Be aware, this part of the script is a WIP so may not catch all images.