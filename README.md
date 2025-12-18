# kub3.uk Talos Cluster Management

This repository contains both the configuration files needed to bootstrap and
manage the Talos Kubernetes clusters within the n3t.uk Lab Environments, but
also the necessary tools to manage upgrades and lifecycle of the clusters too.

> [!note]
> This is currently work-in-progress
> This repository is very much a work in progress still and is being actively
> developed as part of the refresh of the kub3.uk environments.

## Bootstrapping

```console
To start the bootstrapping process, first generate the necessary Talos
configuration files and verify that the information within them is correct for
the target `{environment}` (i.e. `production` or `development`):

$ task clean {environment}:generate
[clean] Completed
[{environment}:generate:talosconfig] generating PKI and tokens
[{environment}:generate:talosconfig] Created talosconfig
[{environment}:generate:talosconfig] Completed
[{environment}:generate:controller-01] generating PKI and tokens
[{environment}:generate:controller-01] Created _build/controller-01.yaml
[{environment}:generate:controller-01] Completed
[{environment}:generate:worker-01] generating PKI and tokens
[{environment}:generate:worker-01] Created _build/worker-01.yaml
[{environment}:generate:worker-01] Completed

```

The completed files can be found under the `{environment}/_build/` directory.

```console
$ talosctl get disks --insecure \
    --nodes controller-01.{environment}.kub3.uk
```

```console
$ talosctl apply-config --insecure \
    --nodes controller-01.{environment}.kub3.uk \
    --file controller-01.yaml
```

```console
$ talosctl dashboard \
    --nodes controller-01.{environment}.kub3.uk
```

```console
$ talosctl bootstrap \
    --endpoints api.{environment}.kub3.uk \
    --nodes controller-01.{environment}.kub3.uk \
    --talosconfig talosconfig
```

```console
$ talosctl apply-config --insecure
    --nodes controller-0{2,3}.{environment}.kub3.uk \
    --file controller-0{2,3}.yaml
$ talosctl apply-config --insecure
    --nodes worker-0{1...9}.{environment}.kub3.uk \
    --file worker-0{1...9}.yaml
```

## Patching

```console
$ talosctl patch mc \
    --nodes worker-01,worker-02,worker-03,worker-04,worker-05,worker-06 \
    --patch @patch-sysctl.yaml
patched MachineConfigs.config.talos.dev/v1alpha1 at the node worker-0{x}
Applied configuration without a reboot
```
