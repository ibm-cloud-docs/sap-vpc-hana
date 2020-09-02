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

# Veeam Backup & Replication overview and considerations
{: #veeam-backup-replication-overview}

Veeam Backup & Replication can be provisioned and deployed from the {{site.data.keyword.cloud}} catalog to provide protection for {{site.data.keyword.cloud_notm}} SAP HANA-certified VMWare and HyperV workloads. It performs SAP `Backint`-based backup and restore of databases and logs.
{: shortdesc}

Figure 1 provides a high-level overview of how an SAP `Backint` database backup is performed.

Figure 1. SAP `Backint` database backup overview

![Figure 1. SAP `Backint` database backup overview](/images/Veeam plug-in.png "SAP `Backint` database backup overview")

Veeam Plug-in, which acts as a go-between for SAP HANA and Veeam Backup repository, is installed on the SAP HANA server. Veeam Backup & Replication is provisioned on either {{site.data.keyword.cloud_notm}} {{site.data.keyword.baremetal_short}} or {{site.data.keyword.cloud_notm}} {{site.data.keyword.virtualmachinesshort}}.

{{site.data.keyword.cloud_notm}} {{site.data.keyword.virtualmachinesshort}} are used for the sample deployment.
{: note}

Veeam Plug-in connects to the Veeam Backup & Replication server and creates a backup job. Once a backup job is requested, Veeam Plug-in starts a Veeam Transport Agent on the SAP HANA server and on the backup repository, which transports data to the backup repository. For more information, see [How Veeam Plug-in for SAP HANA Works](https://helpcenter.veeam.com/docs/backup/plugins/sap_hana_plugin.html?ver=95u4).

## System considerations
{: #veeam-considerations}

Veeam Backup & Replication has a very high-performance data mover engine embedded within its components that is used for
* Reading VM data from production storage
* Applying compression/de-duplication
* Writing the resulting data stream to backup storage

The efficiency that it performs these operations is impacted by available resources, including network bandwidth, storage performance, and server processing capacity.

### Systems requirements
{: #system-requirements}

* **vSphere**. Veeam supports ESXi and vCenter environments v4.1 and greater. While Veaam is suited for backup of individual ESXi hosts, vCenter, if deployed, is the preferred interface into a vSphere environment. vCenter provides added configuration and ease of management.

  Veeam supports all OS types compatible with VMware.

  VMs with disks in the SCSI bus-sharing mode are not supported as VMware does not support snapshotting VMs with those types of disks.

  The following disk types are not supported - RDM virtual disks in physical mode, independent disks, disks connected via in-guest iSCSI initiator, and VMs with pass-through virtual disks. These disk types are skipped from processing automatically.

* **Hyper-V**. Veeam supports Windows Server Hyper-V v2008 R2 SP1 or greater, including
  * Windows Nano Server, with Hyper-V role
  * Free Microsoft Hyper-V Server
  * SCVMM (optional)

  Virtual hardware v5.0 and v8.0, and generation one and two VMs are supported.

* **{{site.data.keyword.cloud_notm}} {{site.data.keyword.virtualmachinesshort}}**. Veeam Backup & Replication components are provisioned on {{site.data.keyword.cloud_notm}} Public {{site.data.keyword.virtualmachinesshort}}. Minimum requirements are
  * 4-core
  * 8 GB RAM
  * 1 Gb network uplink speed

  Veeam supports Windows Server 2012 and later, however, Windows Server 2016 is recommended for the advanced storage optimization and efficiency presented by the Resilient File System (ReFS).

* **Storage**. {{site.data.keyword.IBM_notm}} {{site.data.keyword.blockstorageshort}} Endurance block storage with LUN's from 1 TB to 2 TB, 0.25 IOPS/GB performance level is recommended for hosting the Veeam backup repository. You are encouraged to thoroughly test other performance levels before selecting the performance storage level for your production Veeam repository.

  [The Restore Point Simulator](https://rps.dewin.me/) can be used to help estimate your Veeam backup storage needs.

* **Throughput**. For the initial full backup, Veeam ignores empty or deleted VM storage blocks. For example, if a VM has been allocated 1 TB of storage, thin or thick provisioned, and only consumes 450 G, Veeam only processes 450 G for that VM. For ongoing incremental backups, Veeam only processes VM data blocks changed since the last backup.

  Based on preliminary results, the standard Veeam virtual server processes approximately 220 GB of VM data per hour, which means if total active VM data to be backed up is 1 TB, the initial backup takes approximately 4.5 hours. Based on a 10% per day VM change rate, the daily incremental backup takes approximately 45 minutes.

## References
{: #veeam-references}

* [Veeam Backup & Replication Evaluator's Guide for VMware vSphere](https://helpcenter.veeam.com/evaluation/backup/vsphere/en/getting_started.html)
* [Veeam Backup & Replication Evaluator's Guide for Microsoft Hyper-V](https://helpcenter.veeam.com/evaluation/backup/hyperv/en/getting_started.html)
* [Veeam Support Knowledge Base](https://www.veeam.com/kb_search_results.html)
* [Veeam Backup & Replication Best Practices](https://bp.veeam.expert/)

## Next Steps
{: #hana-veeam-overview-next-steps}

  [1. Provisioning the {{site.data.keyword.cloud_notm}} infrastructure to host Veeam Backup & Replication](/docs/sap-vpc-hana?topic=sap-vpc-hana-veeam-provisioning-infrastructure)

  [2. Configuring the server for Veeam Backup & Replication](/docs/sap-vpc-hana?topic=sap-vpc-hana-veeam-configuring-server)

  [3. Creating the Veeam repository](/docs/sap-vpc-hana?topic=sap-vpc-hana-veeam-creating-repository)

  [4. Submitting the initial backup job](/docs/sap-vpc-hana?topic=sap-vpc-hana-veeam-submitting-backup-job)
