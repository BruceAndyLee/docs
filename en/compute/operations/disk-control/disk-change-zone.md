---
title: "How to move a disk to a different availability zone in {{ compute-full-name }}"
description: "Follow this guide to move a disk to a different availability zone."
---

# Moving a disk to a different availability zone

A disk is created in a single [availability zone](../../../overview/concepts/geo-scope.md). You can move it to a different zone using a snapshot or CLI command.

If a disk is attached to a VM, it will be moved during [VM migration](../vm-control/vm-change-zone.md). You cannot migrate attached disks separately from their VMs.

{% include [relocate-note](../../../_includes/compute/relocate-note.md) %}

## Moving a disk by creating a snapshot {#relocate-via-snapshot}

### Prepare the disks {#prepare-disks}

{% include [prepare-snapshots](../../../_includes/compute/prepare-snapshots.md) %}

### Create snapshots {#create}

To [create](../disk-control/create-snapshot.md) a disk snapshot:

{% include [create-snapshot](../../../_includes/compute/create-snapshot.md) %}

Repeat the steps to create a snapshot of each disk.

### Create a VM in a different availability zone with the disks from the snapshots {#create-vm}

To [create](../vm-create/create-from-snapshots.md) a VM in a different availability zone with the disks from the snapshots:

{% include [create-from-snapshot](../../../_includes/compute/create-from-snapshot.md) %}

## Moving a disk using the CLI {#relocate-cli}

{% note info %}

If data is being written to the disk, its move may fail. In this case, stop the disk write operation or shut down the VM and restart the disk move.

{% endnote %}

To attach the disk to a VM from a different availability zone, move it:

{% list tabs %}

- CLI

   {% include [cli-install](../../../_includes/cli-install.md) %}

   {% include [default-catalogue](../../../_includes/default-catalogue.md) %}

   1. View the description of the CLI disk move command:

      ```bash
      {{ yc-compute }} disk relocate --help
      ```

   1. Request a list of available disks:

      {% include [compute-disk-list](../../../_includes/compute/disk-list.md) %}

   1. Select the `ID` of the appropriate disk.

   1. Move the disk to a different availability zone:

      ```bash
      {{ yc-compute }} disk relocate --id <disk_ID> \
        --destination-zone-id <availability_zone>
      ```

      Where:

      * `--id`: ID of the disk to be moved.
      * `--destination-zone-id`: ID of the availability zone to move the disk to, e.g., `{{ region-id }}-a`.

      If you are moving a [disk in a placement group](../../concepts/disk-placement-group.md), provide the group ID in the `--disk-placement-group-id` parameter.

{% endlist %}

