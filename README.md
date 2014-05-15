# Yandex cocaine cloud for Vagrant

## Requirements

* Virtualbox
* Vagrant
* Ansible

## Usage

By default, all provision can be done by following command:

    vagrant up

## Boxes

There are five boxes.

* `elliptics` holds one node of Reverbrain Elliptics. All other boxes uses
Elliptics as storage.
* `cocaine01` and `cocaine02` runs `cocaine-runtime`. Also, `cocaine-tools`
installed on both boxes
* `front` runs `cocaine-runtime` in gateway mode and `cocaine-native-proxy`
on port `8080`.
* `client` holds only `cocaine-tools`. Use for run isolated scripts.

## Cluster control

To control cluster use `cluster.yaml`.

## Network

For communication