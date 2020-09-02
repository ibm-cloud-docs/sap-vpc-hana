---

copyright:
  years: 2020
lastupdated: "2020-09-02"

keywords: SAP HANA, {{site.data.keyword.cloud_notm}} storage, {{site.data.keyword.blockstorageshort}}, Veeam Backup & Replication, SAP Backint, Veeam Plug-in

subcollection: sap-vpc-hana

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:external: target="_blank" .external}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:note: .note}

# Provisioning the IBM Cloud infrastructure to host Veeam Backup & Replication
{: #veeam-provisioning-infrastructure}

You need to provision an {{site.data.keyword.IBM}} Virtual Server and {{site.data.keyword.blockstorageshort}} to host Veeam Backup & Replication.
{: shortdesc}

## Provisioning a Virtual Server to host your Veeam Backup & Replication instance
{: #provisioning-vsi}

Use the following steps to provision the virtual server that will host your Veeam Backup & Replication instance. For more information on provisioning public virtual server instances, see [Provisioning public instances](/docs/virtual-servers?topic=virtual-servers-ordering-vs-public).

1. Log in to the [{{site.data.keyword.cloud_notm}} console](https://cloud.ibm.com/){: external} with your unique credentials.
2. Click **Create resource** > **Compute** > **Infrastructure** > **Virtual Server**.
3. The default is **Public Virtual Server**. Change the type to **Starting from $25.00 monthly** and click **Continue**.
4. Leave the defaults for **Type of virtual server**, **Quantity**, and **Billing**.
5. **Hostname** is a permanent or temporary name for your servers, for example, `virtualserver01`. Hover over **Information** for formatting specifics.
6. **Domain** is the identification string that defines administrative control within the internet, for example, `Customer-145840.cloud`. Hover over **Information** for formatting specifics.
7. Leave **Placement group** set to **None**.
8. Select the **Location** where your current SAP-certified infrastructure is located.
9. Click **All profiles** and select **B1.4x8**. You can select a larger configuration if your needs require it.
10. Click **Microsoft Image** (OS) and select **Windows 2016 Standard (64 bit)-HVM**.
11. Click **Add-ons** (under Image) and select **Veeam**, and choose your Veeam licensing based on the number of servers or SAP HANA nodes to be backed up.
12. Leave **Attached storage disks** as is.
13. Change **Uplink port speeds** to **1 Gbps Public & Private Network Uplinks**.
14. Leave the default values for all other fields.
15. Review your Order Summary.
16. Click _I have read and agree to Third-Party Service Agreements_.
17. Click **Create** to be redirected to the Checkout page after your order has been verified.

You are redirected to a page with your order number. You can print the page, because it's your receipt. In addition, you receive a confirmation email with the subject *Your IBM Cloud Order ## has been approved* with ## being your order number.

After the order is submitted, the server, depending on your order, is available for use within one to four hours. You can check Device Details from the {{site.data.keyword.cloud_notm}} console (Menu icon ![Menu icon](../../icons/icon_hamburger.svg) > Resource List > Devices) for a status of the provisioning steps. Click the **Device Name** that matches your given Hostname and Domain to see its status.

## Provisioning {{site.data.keyword.blockstorageshort}} to host your Veeam Backup & Replication backup repository
{: #provisioning-block-storage}

1. Expand the Menu icon ![Menu icon](../../icons/icon_hamburger.svg) and select *Classic Infrastructure*.
2. Select **Storage** > **Block Storage** > **Order Block Storage**.
3. Select the specifics for your storage needs. Table 1 contains the recommended values for your backup repository.
|              Field               |      Value                                                 |
| -------------------------------- | ---------------------------------------------------------- |
|Location                          | Same as where you provisioned your virtual server instance |
|Billing Method                    | Monthly (default)                                          |
|Size                              | 1,000 GB                                                   |
|Endurance (IOPS tiers)            | 0.25 IOPS/GB                                               |
|Snapshot space                    | 0 GB                                                       |
|OS Type                           | Windows 2008+                                              |
{: caption="Table 1. Recommended values for block storage" caption-side="top"}
4. Click **Create**.

### Authorizing host
{: #authorizing-hosts-console}

1. Select **Storage** > **Block Storage**.
2. Highlight your LUN and expand the Action menu ![Action menu](../../icons/action-menu-icon.svg) and select **Authorize Host**.
3. Select a **Device Type** of **Virtual Server**.
4. Click **Virtual Guest** and select the name of the server you provisioned for Veeam Backup & Replication.
5. Click **Save**.

  Additional provisioning information can be found under [Ordering Block Storage through the Console](/docs/BlockStorage?topic=BlockStorage-orderingthroughConsole).
  {: tip}  

## Next steps
{: #hana-veeam-provision-next-steps}

[2. Configuring the server for Veeam Backup & Replication](/docs/sap-vpc-hana?topic=sap-vpc-hana-veeam-configuring-server)

[3. Creating the Veeam repository](/docs/sap-vpc-hana?topic=sap-vpc-hana-veeam-creating-repository)

[4. Submitting the initial backup job](/docs/sap-vpc-hana?topic=sap-vpc-hana-veeam-submitting-backup-job)
