Ada is a Kubernetes cluster running k3s on Fedora core-os nodes.

Each node is configured using core-os' Butane/Ignition configuration format.
A central file `ada.bu` is used to create a common configuration.
`makefile` is then used to create the per-node configurations as `adaN.ign` files.

Deployment requires every node to be booted into a fedora core-os live ISO.
This provides an installation command which can be used to flash the host's OS.
Upon first boot, the host will self configure.

Note: if the provided config file includes systemd services, initial setup may involve multiple reboots.
