---
title: VM Snapshot extension using VM restore points
description: VM Snapshot extension using VM restore points
author: aarthiv
ms.author: aarthiv
ms.service: virtual-machines
ms.subservice: recovery
ms.topic: how-to
ms.date: 11/2/2023
ms.custom: template-how-to
---

# VMSnapshot extension

Application-consistent restore points use VSS service (or pre/post-scripts for Linux) to verify application data consistency prior to creating a restore point. Achieving an application-consistent restore point involves the VM's running application providing a VSS service (for Windows) or pre- and post-scripts (for Linux).

For Windows images, **VMSnapshot Windows** extension and for Linux images, **VMSnapshot Linux** extension is used for taking application consistent restore points. When there's a create application consistent restore point request issued from a VM, Azure installs the VM snapshot extension if not already present. The extension will be updated automatically.

> [!IMPORTANT]
> Azure will begin creating a restore point only after all extensions (including but not limited to VMSnapshot) provisioning state are complete.

## Extension logs

You can view logs for the VMSnapshot extension on the VM under 
```C:\WindowsAzure\Logs\Plugins\Microsoft.Azure.RecoveryServices.VMSnapshot``` for Windows  and under ```/var/log/azure/Microsoft.Azure.RecoveryServices.VMSnapshotLinux/extension.log``` for Linux.


## Troubleshooting

Most common restore points failures are attributed to the communication with the VM agent and extension, and can be resolved by following the troubleshooting steps listed in the [troubleshooting](restore-point-troubleshooting.md) article.

During certain VSS writer failure, Azure takes a file system consistent restore points consequently for next three times (irrespective of the frequency at which the restore point creation is scheduled) upon failing the initial creation request. From the fourth time onwards an application consistent restore point will be attempted. 

Follow these steps to [troubleshoot VSS writer issues](../backup/backup-azure-vms-troubleshoot.md#extensionfailedvsswriterinbadstate---snapshot-operation-failed-because-vss-writers-were-in-a-bad-state).

> [!NOTE]
> Avoid manually deleting the extension, as it will lead to failure of the subsequent creation of an application-consistent restore point

## Next steps

- [Create a VM restore point](create-restore-points.md).