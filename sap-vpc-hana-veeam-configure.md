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

# Configuring the server for Veeam Backup & Replication
{: #veeam-configuring-server}

## Before you begin
{: #hana-veeam-configure-before-you-begin}

Before you configure your server to host Veeam Backup & Replication, make sure the following prerequisites have been met.

* {{site.data.keyword.baremetal_long}} or public {{site.data.keyword.virtualmachineslong}} have been provisioned
* {{site.data.keyword.cloud_notm}} {{site.data.keyword.blockstorageshort}} has been allocated to the Veeam backup repository
* You have credentials to access your hypervisor environment
{: shortdesc}

## Attaching {{site.data.keyword.blockstorageshort}} to your provisioned servers
{: #attaching-block-storage-for-veeam}

Use the following steps to enable {{site.data.keyword.cloud_notm}} {{site.data.keyword.blockstorageshort}} multipath I/O (MPIO) on your provisioned server.

1. Log in to {{site.data.keyword.cloud_notm}} console and access your server through the Device List. If you need help accessing the Device List, see [Navigating to devices](docs/virtual-servers?topic=virtual-servers-navigating-devices).
2. Open Windows Server Manager and select **Local Server**.
3. Under **Manage**, choose **Tasks** > **Add Roles and Features**. Click **Next** four times.
4. Click **Multipath I/O** under Features > **Next**. Select **Restart the destination server automatically** > **Install**.
5. A feature installation confirmation should appear to validate the installation of MPIO.

### Configure Windows iSCSI Initiator
{: #configure-windows-iscsi-initiator}

You'll be going between the {{site.data.keyword.cloud_notm}} console and the Windows Server Manager dashboard to configure the Windows iSCSI Initiator.
{: note}

1. Click the Menu icon ![Menu icon](../../icons/icon_hamburger.svg) > **Classic Infrastructure** > **Storage** > **Block Storage** and select your {{site.data.keyword.blockstorageshort}} LUN.
2. Under **Authorized Hosts**, highlight and copy the LUN's **HOST IQN**.
3. Go back to the Windows Server Manager dashboard and select **Tools** > **iSCSI Initiator**.
4. Click the **Configuration** tab in the iSCSI Initiator Properties window, click **Change**, paste the Host IQN into **New initiator name**, and click **OK**.
5. Restore the {{site.data.keyword.cloud_notm}} console and copy a **Target Address**. Either address will work.
6. Restore the iSCSI Initiator Properties, click **Discovery** > **Discover Portal**, and paste the Target Address into **IP address or DNS name**. Click **Advanced**.
7. Go back to the {{site.data.keyword.cloud_notm}} console and copy **Username** under Authorized Hosts.
8. Restore the iSCSI Initiator Properties, click **Enable CHAP log on**, and paste the Username in **Name** under Advanced Settings.
9. Go back to the {{site.data.keyword.cloud_notm}} console and copy **Password** under Authorized Hosts.
10. Restore the iSCSI Initiator Properties, paste the Username in **Target secret** under Advanced Settings.
11. Click **OK** twice.
12. Select **Targets** and click the inactive Discovered Storage IQN. Click **Connect** and click **OK**.
13. Click **Advanced** and repeat steps 7 to 11.
14. Minimize the {{site.data.keyword.cloud_notm}} console. You'll need it when you establish a connection between the Veeam backup repository and infrastructure.

  The storage target is now in a _Connected_ status if all settings were correctly entered.

### Enabling operating system visibility to the attached storage
{: #enabling-os-visibility}

1. Go back to the Windows Server Manager dashboard and select **Tools** > **Computer Management**, and click **OK** to initialize the new disk.
2. Select **Storage** > **Disk Management**. The new storage disk will be offline.
3. Right-click on the disk and click **Online**. You will also need to right-click and initialize any storage not previously in use.
4. Right-click in the volume field and click **New Simple Volume**. The New Simple Volume Wizard will display. Click **Next**.
5. Select the following, click **Next**, and click **Finish** when you're done.

| Field | Value |
| --- | --- |
| Simple volume size in MB                      | Select the maximum amount                          |
| Assign the following drive letter             | Accept the default value                           |
| Format the volume with the following settings | Select                                             |
| File system                                   | ReFS                                               |
| Allocation unit size                          | 64K                                                |
| Volume name                                   | Enter a name for the volume, for example `VBRrepo` |

  64K block size is selected because the sample VSI is running Windows 2016 as its OS. The selected settings help ensure optimal performance and reliability with your Veeam deployment.

  The volume is now online.

## Next steps
{: #hana-veeam-configure-next-steps}

  [3. Creating the Veeam repository](/docs/sap-vpc-hana?topic=sap-vpc-hana-veeam-creating-repository)

  [4. Submitting the initial backup job](/docs/sap-vpc-hana?topic=sap-vpc-hana-veeam-submitting-backup-job)
