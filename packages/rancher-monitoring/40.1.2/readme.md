# kube-state-metrics 40.1.2

This must be built with `CHARTS_BUILD_SCRIPT_VERSION=v0.3.3` set otherwise it will fail.
Specifically you want to run:
```bash
CHARTS_BUILD_SCRIPT_VERSION=v0.3.3 PACKAGE=rancher-monitoring/40.1.2 make charts
```

Due to that this specific version will probably be better to remove - or update for new chart scripts.
We need it initially though as a way to prove the new charts process works as expected.