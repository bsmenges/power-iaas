﻿---

copyright:
  years: 2019, 2020

lastupdated: "2020-04-10"

keywords: network interface, NIC, AIX VM, ifconfig command, detach, en0, rmdev, external IP address, smitty mktcpip, namerslv command

subcollection: power-iaas

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:important: .important}
{:note: .note}
{:external: .external}
{:help: data-hd-content-type='help'}
{:support: data-reuse='support'}

# How to add or remove a network interface from an AIX virtual machine (VM)
{: #managing-network-interface}

Since IBM PowerVC Version 1.2.2, IBM PowerVC can dynamically add a network interface controller (NIC) to a VM or remove a NIC from a VM. IBM PowerVC does not set the IP address for new network interfaces that are created after the machine deployment. Any removal of a NIC results in freeing the IP address that was set on it.  You must remove and readd the AIX VM network interface if you choose to disconnect the {{site.data.keyword.powerSys_notm}} AIX VM from a public network.
{: shortdesc}

When you toggle a public network off and then on, the {{site.data.keyword.powerSys_notm}} user interface regenerates new internal and external IP addresses. You need to check the {{site.data.keyword.powerSys_notm}} user interface for the new internal IP address to complete this procedure.
{: note}

## Removing a network interface from an AIX VM
{: #remove-nic-aix}
{: help}
{: support}

1. Use the `ifconfig` command to remove the network interface from the AIX VM. In the following example, *en0* is the public interface.

    ```
    ifconfig en0 down detach
    ```
    {: codeblock}

2. Next, run the `rmdev` command to remove the device from the AIX system.

    ```
    rmdev -dl en0
    ```
    {: codeblock}

3. Use the `cfgmgr` command to reconfigure the device.

## Adding a network interface to an AIX VM
{: #add-nic}

When you toggle a public network off and then on, a new Virtual Ethernet Adapter (VEA) is created on your VM. You must identify the network interface that the new VLAN is associated with before reconfiguring the IP adress.

Every time you toggle a public network off and then on, the system creates multiple network interfaces. As a result, the `ifconfig -a` command will show several unused network interfaces after toggling the public network. To remove these stale network interfaces, use `ifconfig` and `rmdev` as described in the previous section.
{: note}

1. After you toggle a public network off and then on, look up the list of networks and write down the public network VLAN ID. To find the public network VLAN ID, see [Get a network's current state or information](https://cloud.ibm.com/apidocs/power-cloud#get-a-network-s-current-state-or-information){: new_window}{: external}.

2. Use the `lsdev -Cc adapter` command to generate the list of adapters.

3. Use either the `ifconfig -a` or `netstat -in` command to see all of the network interfaces.

4. To find the network interface that has the public network VLAN ID, enter `entstat -d ent X | grep VLAN` (where *X* is the adapter number).

To add a network interface (for example, *en0*) and point it to the new internal IP address (as shown in the {{site.data.keyword.powerSys_notm}} user interface), you can use `smitty mktcpip`. You can also use the AIX command line to perform the same task by using the [mktcpip command](https://www.ibm.com/support/knowledgecenter/en/ssw_aix_72/m_commands/mktcpip.html){: new_window}{: external} (replacing the values with your own):

```
/usr/sbin/mktcpip -h power-systems-virtual-instance -a 192.168.103.12 -m 255.255.255.240 -i en0 -t N/A -g 192.168.103.1 -D 0.0.0.0
```
{: codeblock}

If you'd like to manipulate domain name server (DNS) entries for local resolver routines in the system configuration database, see the [namerslv command](https://www.ibm.com/support/knowledgecenter/ssw_aix_72/n_commands/namerslv.html){: new_window}{: external}.

