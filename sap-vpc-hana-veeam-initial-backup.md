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

# Submitting the initial backup job
{: #veeam-submitting-backup-job}

Once the infrastructure and repository has been set up, you're ready to create and submit your initial backup job.
{: shortdesc}

## Before you begin
{: #initial-backup-before-you-begin}

Be sure to have set up the correct permissions on the backup repository. For instructions on setting up permissions, see [Granting Permissions on Repositories](https://helpcenter.veeam.com/docs/backup/plugins/repository_permissions.html?ver=95u4)
{: important}

## Creating the initial backup job
{: #creating-initial-backup-job}

1. From the Veeam Backup & Replication console, select **Backup & Replication** > **Jobs** > **Backup Job**. The **New Backup Job** wizard displays.
2. Under Add Objects, select the IP address of the server you provisioned for Veeam Backup & Replication. Click **Add**,

   Add Objects will list options for the VMs to include in the backup job. Individual VMs, VM folders, VM resource pools, or entire hosts and clusters can be added to the backup job. Adding VM groups simplifies ongoing job maintenance as the Veeam backup job doesn't need to be modified in the event VMs are added or removed from the grouping entity.
   {: note}

3. Under Storage in the New Backup Job wizard, **Backup proxy** defaults to **Automatic selection**. For **Backup repository**, select the name of the scale-out proxy and click **Next**.

   You're selecting the scale-out backup repository for the job's backup storage target because by default 14 restore points (days) are maintained. These default restore points mean that if a job runs daily, two weeks worth of backups are maintained. The backup chain is constructed using _forward incremental backups_. The initial full backup is stored and only changed blocks are stored for the subsequent restore points. For more information, see [Backup Chain](https://helpcenter.veeam.com/archive/backup/95/vsphere/backup_files.html).
   {: note}

4. Click **Next** under Guest Processing.
5. Select **Daily at this time** at **8:00 AM** for your schedule.
6. Click **Apply**.
7. Click **Run the job when I click Finish** to run the initial backup job immediately. Click **Finish**.
8. Double-click the running job and select **Show Details** to see granular status information on the running backup job.

For information on Veeam's application-aware processing for advanced application-specific item level recovery for SQL, Oracle, Exchange, SharePoint, and others, see [Application-Aware Processing](https://helpcenter.veeam.com/archive/backup/95/vsphere/application_aware_processing.html).   
