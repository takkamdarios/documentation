---
title: "Editing a Physical Interface"
description: "Provides intstructions on how to edit a network physical interface on TrueNAS CORE."
weight: 50
aliases: /core/network/interfaces/editingphysicalinterface/
tags:
- network
- interfaces
---

{{<toc>}}

## Interface Editing ###

{{< hint type=important >}}
Be careful when configuring the network interface that controls the TrueNAS® web interface. An error can result in the loss of web connectivity.
{{< /hint >}}

**Network > Interfaces** lists all the physical [Network Interface Controllers (NICs)]({{< relref "/CORE/UIReference/Network/InterfacesScreen.md" >}}) connected
to your TrueNAS® system.

![NetworkInterfaces](/images/CORE/Network/NetworkInterfaces.png "Interfaces List")

To edit an interface, click **>** next to it to expand the view. This provides a general description about the chosen interface. Click *EDIT*.

{{< hint type=note >}}
TrueNAS Enterprise customers: you cannot edit an interface with High Availability (HA) enabled.  
Go to **System > Failover** and check the **Disable Failover** box, then click **SAVE**.
{{< /hint >}}

![NetworkInterfaceDescription](/images/CORE/Network/NetworkInterfaceDescriptionView.png "Network Interface Description")

{{< hint type=note >}}
The **Type** of interface determines the interface editing options available.
{{< /hint >}}

See [Interfaces Screen]({{< relref "/CORE/UIReference/Network/InterfacesScreen.md" >}}) for more information on settings.

## Saving Changes ##

After you're done editing, click **SAVE**. You have the option to **TEST CHANGES** or **REVERT CHANGES**. The default time for testing any changes is 60 seconds, but you can change it to your desired setting.  

![NetworkInterfacesChangesPresent](/images/CORE/Network/NetworkInterfacesChangesPresent.png "Interface Changes Detected")

After clicking **TEST CHANGES**, confirm your choice and click **TEST CHANGES** again.

![NetworkInterfaceTestChangesNotice](/images/CORE/Network/NetworkInterfaceTestChangesNotice.png "Network Interface Test Changes Notice")

Users can either **SAVE CHANGES** or **REVERT CHANGES**. A user has the time they specified to make their choice. If you select **SAVE CHANGES**, a dialog box asks you to **CANCEL** or **SAVE** network interface changes. Click **SAVE**.

![NetworkInterfaceEditSaveChanges](/images/CORE/Network/NetworkInterfaceEditSaveChanges.png "Network Interface Edit Save Changes ")

![NetworkInterfaceSaveChangesOption](/images/CORE/Network/NetworkInterfaceSaveChangesOption.png "Network Interface Save Changes Option ")

The system displays a dialog box to show that network interface changes are now permanent.

![NetworkInterfaceDialog](/images/CORE/Network/NetworkInterfaceDialogBox.png "Network Interface Dialog Box ")
