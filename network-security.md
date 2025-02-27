---

copyright:
  years: 2019, 2020

lastupdated: "2020-01-23"

keywords: firewall, ports, network security, vSRX, ICMP

subcollection: power-iaas

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:deprecated: .deprecated}
{:external: target="_blank" .external}

# Network security
{: #network-security}

Infrastructure provides virtual LAN (VLAN) isolation between different tenants, which are enforced at Virtual I/O Server (VIOS) and physical switches and routers.
{: shortdesc}

## Default firewall ports
{: #firewall-ports}

The {{site.data.keyword.powerSysShort}} network security architecture relies on a set of fixed firewall ports open on the *Juniper vSRX* firewalls:
{: shortdesc}

There are plans to add the ability to dynamically configure the firewall rules in the future.
{: note}

* 22 (SSH)
* 443 (HTTPS)
* 992 (IBM i5250 emulation SSL)
* ICMP traffic

The following firewall ports are also open, typically used for IBM i logical partitions (LPARs):

* 2005
* 2007
* 2010
* 2012
* 9470
* 9475
* 9476

The port 6443 is also open for miscellaneous purposes. This port will not be open for the WDC04 and DAL13 data centers.

If you need extra ports to be opened, you can consider customer-specific firewall option that is currently available by using an IBM Cloud firewall, such as Vyatta, Juniper vSRX, or FortiGate, and by connecting to Power Systems™ Virtual Server by using [Direct Link Connect](/docs/power-iaas?topic=power-iaas-ordering-direct-link-connect). To understand the Power Systems™ Virtual Server connection methods, see [Network architecture diagrams](/docs/power-iaas?topic=power-iaas-network-reference-architecture).
