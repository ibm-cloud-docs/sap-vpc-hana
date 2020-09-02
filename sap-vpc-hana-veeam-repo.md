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
{:important: .important}

# Creating the Veeam repository
{: #veeam-creating-repository}

The next step in implementing Veeam Backup & Replication is to create repository and establishing connections to your infrastructure.
{: shortdesc}

## Setting up the repository
{: #setting-up-repo}

Use the following steps to set up the Veeam repository.

1. Log in to the Veeam Backup & Replication console.
2. Select **Backup Infrastructure** > **Backup Repository** > **Add Repository**.
3. Click **Direct Attached Storage** > **Microsoft Windows server**.
4. Enter your repository's **Name**, for example, `RSL025IOPS`, **Descirption**, and click **Next**.
5. Click **Populate** and select the drive that corresponds to the attached {{site.data.keyword.blockstoragefull}}. For the example, the corresponding drive is `V:\`. Click **Next**.
6. Click **Advanced**, select **User per-VM backup files**, and click **OK**.
7. Click **Next** twice and click **Apply** to apply the settings.
8. Click **Finish** to create the repository extent, then click **No** to relocate the Veeam configuration backup location to the new repository.

To be able to store backups on a Veeam backup repository, accounts, including administrative accounts, _must_ have access permissions on the backup repository. For instructions on setting up permissions, see [Granting Permissions on Repositories](https://helpcenter.veeam.com/docs/backup/plugins/repository_permissions.html?ver=95u4)
{: important}

## Creating a backup repository
{: #creating-backup-repo}

You can now begin writing backups to the new repository. However, for maximum economy, flexibility, and scalability, it is strongly advised to create an optional scale-out backup repository. Why should you create an optional scale-out backup repository? For economy, it enables the purchase of only the {{site.data.keyword.blockstorageshort}} you need for your current deployment. As your {{site.data.keyword.cloud_notm}} infrastructure grows, additional {{site.data.keyword.blockstorageshort}} `extents` can be purchased and added to the scale-out backup repository. You lessen your up-front costs for the Veeam solution without incurring the overhead of reconfiguring backup jobs to point to specific `extents` within the repository.

Use the following steps to create a scale-out backup repository.

1. From the Veeam Backup & Replication console, select **Scale-out Repositories** > **Add Repository**.
2. Enter a **Name** and **Description** for the scale-out repository, and click **Next**.
3. Click **Add** and select the new backup repository. Click **Apply**.

## Establishing a connection to the {{site.data.keyword.cloud_notm}} Infrastructure
{: #establishing-connection-to-infrastructure}

Now that you've created your backup repository, you need to connect the repository to the {{site.data.keyword.cloud_notm}} infrastructure. Use the following steps to establish the connection.

1. From the Veeam Backup & Replication console, select **Backup Infrastructure** > **Managed Servers** > **Add Server** > **MICROSOFT SERVER**.
2. Enter a **DNS name or IP address** and **Description** for your ESXi host, vCenter, or Hyper-V server. Click **Next**.
3. Restore the {{site.data.keyword.cloud_notm}} console and go to [Menu icon ![Menu icon](../../icons/icon_hamburger.svg) > **Resource List** > **Devices** and find the server you set up for Veeam Backup & Replication.
4. Copy the Username and go back to the Veeam Backup & Replication console. Paste the server username in **Credentials Username**.
5. Go back to the {{site.data.keyword.cloud_notm}} console and copy the server's Password.
6. Restore the Veeam Backup & Replication console and paste the server password in **Credentials Password**. Enter a **Description** and click **OK**.
7. Click **Connect** to the Security Warning, and click **Next**.

The server should now appear under Managed Servers.

## Next Steps
{: #hana-veeam-repo-next-steps}

[4. Submitting the initial backup job](/docs/sap-vpc-hana?topic=sap-vpc-hana-veeam-submitting-backup-job)
