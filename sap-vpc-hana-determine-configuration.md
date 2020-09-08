---

copyright:
  years: 2020
lastupdated: "2020-09-02"

keywords: SAP HANA, {{site.data.keyword.cloud_notm}}, RDBMS, SAP Application Performance Standards, SAPS, SAP Certified, database

subcollection: sap-vpc-hana

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:external="_blank" .external}
{:pre: .pre}
{:tip: .tip}
{:table: .aria-labeledby="caption"}


# Determining your configuration
{: #determine_configuration}

With an SAP HANA system, you have two deployment options, single-node and multi-node. With Gen2 of IBM Cloud, in a first stage only single node systems are supported.

However, these single-node systems also have a strong dependency on their storage layout so that they can comply to the Tailored Datacenter Integration version 5 (TDI v5) KPIs defined by SAP and thus provide optimal performance of SAP HANA in customer environments, not only in cloud environments.
{:shortdesc}

The following method of creating physical and logical volumes as well as sizing the file systems, is just one possible   approach. For production use, you might want to follow the specific sizing information for your system. Other layouts might better meet your needs or requirements for your SAP HANA workload.

  In order to fulfill the KPIs defined by SAP, this method will give the exact definition of the storage volumes that need to be created. If you prefer to have a different storage layout, please make sure that SAP's TDIv5 storage KPIs are met by running the required tests. For further information see [TDI overview](https://www.sap.com/documents/2017/09/e6519450-d47c-0010-82c7-eda71af511fa.html){: external} and [TDI FAQ](https://www.sap.com/documents/2016/05/e8705aae-717c-0010-82c7-eda71af511fa.html){: external}.
  {: note}

## Initial Steps to prepare the SAP HANA VSI

To patch your virtual server instance operating system to the latest level, run `yum update` and restart the virtual server instance.

Many SAP and other installation tools such as SAP NetWeaver Software Provisioning Manager (SWPM), don't allow products to install to hostnames that don't resolve to an external IP address on the server instance. Because of this, the default settings in the virtual server instance need to be adapted.

To change these settings, complete these steps:
1. Edit **`/etc/hosts`** and comment out the lines that resolve the hostname to the IPv4 AND IPv6 localhost addresses.

2. Add lines that resolve to the external IP address of your virtual server. In our example, the hostname resolves to `10.242.128.8`, the private IP displayed in Figure 6. In this example, we append a sample default domain. You should adapt this example to your specific environment.

Following are samples of the **`/etc/hosts`** file, one for IPv4 and one for IPv6.

For IPv4, make sure that `127.0.0.1 sap-hana-vsi sap-hana-vsi` is commented out or deleted. For IPv6, make sure that `#::1 sap-hana-vsi sap-hana-vsi` is commented out or deleted.

```
# The following lines are desirable for IPv4 capable hosts

#127.0.0.1 sap-hana-vsi sap-hana-vsi
127.0.0.1 localhost.localdomain localhost
127.0.0.1 localhost4.localdomain4 localhost4

# The following lines are desirable for IPv6 capable hosts

#::1 sap-hana-vsi sap-hana-vsi
::1 localhost.localdomain localhost
::1 localhost6.localdomain6 localhost6
10.243.128.8 sap-hana-vsi.saptest.com sap-hana-vsi
```

To prevent the {{site.data.keyword.cloud_notm}} `cloudinit` process from reverting back the content of **`/etc/hosts`** to the previous values on the next restart, change the configuration in `/etc/cloud/cloud.cfg`. Change `manage_etc_host` from `True` to `False`.

Install Logical Volume Manager (LVM) to make the related command available that will be used below.
```
[root@sap-hana-vsi ~]# yum install lvm2*
```


## Storage configuration for SAP HANA VSIs
{: #storage_config}

Table 1 is a sample storage configuration for an **mx2-32x256** virtual server instance that uses [{{site.data.keyword.block_storage_is_full}}](/docs/vpc?topic=vpc-block-storage-about) volumes with the following:

  * 500 GB storage at 10,000 IOPS for the database
  * 2,000 GB for backup at 4,000 IOPS (medium performance)

| File system | Volume | Storage type | IOPS/GB | GB | \# IOPS |
| --- | --- | --- | --- | --- | --- |
| `/` | `vda1` | Pre-configured boot volume | N/A | 100 GB | 3,000 |
| `/boot` | `vda2` | Pre-configured boot volume | N/A | 0.25 GB | 3,000 |
{: caption="Table 1. Storage configuration of the default VSI deployment (boot volume)" caption-side="top"}

Table 1 shows a basic layout of the file system to support. To fulfill the size and I/O requirements, more block storage volumes need to be added to the virtual server configuration as data volumes.

Block Storage Volumes for VSIs can be created based on different **volume profiles** providing different levels of IOPS per gigabyte (IOPS/GB), see [IOPS tiers](/docs/vpc?topic=vpc-block-storage-profiles#tiers) for details. However, in order to fulfill the KPIs defined by SAP, this chapter will give the exact definition of the storage volumes that need to be created.

### Storage for mx2-16x128 and mx2-32x256 Profile-based VSIs

For VSIs created based on the **mx2-16x128** and **mx2-32x256** profiles, three block storage volumes of 500 GB size, with a custom volume profile that supports up to 10,000 **Max IOPS** need to be attached to the VSI. After attaching the three data volumes, three new virtual disks will appear in the VSI, see the table that follows. In our sample deployment, those disks are `vdd`, `vde` and `vdf`.

| Volume Group | Volume | Storage type | IOPS/GB | GB |
| --- | --- | --- | --- | --- |
| `hana_vg` | `vdd` | data volume | 10,000 | 500 GB
| `hana_vg` | `vde` | data volume | 10,000 | 500 GB
| `hana_vg` | `vdf` | data volume | 10,000 | 500 GB
{: caption="Table 2. Storage for mx2-16x128 and mx2-32x256 profile based VSIs" caption-side="top"}

The disks are visible in the operating system of the VSI as follows:

```
[root@hana256-vsi ~]# fdisk -l
Disk /dev/vdd: 536.9 GB, 536870912000 bytes, 1048576000 sectors
Units = sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes


Disk /dev/vde: 536.9 GB, 536870912000 bytes, 1048576000 sectors
Units = sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes


Disk /dev/vdf: 536.9 GB, 536870912000 bytes, 1048576000 sectors
Units = sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
```

These three disks must be managed under the Linux&reg; Logical Volume Manager (LVM), and deployed as logical volumes. In order to achieve that, first put the three devices under LVM control. For example, make them physical volumes:

```
[root@hana256-vsi ~]# pvcreate /dev/vdd /dev/vde /dev/vdf
```

Then, create a volume group from the physical volumes. The name of the volume group can be chosen according to your preferences, in our sample it is `hana_vg`:

```
[root@hana256-vsi ~]# vgcreate hana_vg /dev/vdd /dev/vde /dev/vdf
```

After creating the volume group, three logical volumes need to be defined on top. These logical volumes reflect the file system size requirements for SAP HANA. The following commands are for a 256 GB VSI - in a 128 GB VSI, in the example below `-L 256GB` must be replaced by `-L 128GB`.

```
[root@hana256-vsi ~]# lvcreate -i 3 -I 64K -L 256GB -n hana_log_lv hana_vg
[root@hana256-vsi ~]# lvcreate -i 3 -I 64K -L 256GB -n hana_shared_lv hana_vg
[root@hana256-vsi ~]# lvcreate -i 3 -I 64K -l 100%FREE -n hana_data_lv hana_vg
```

These commands will not result in the smallest possible file system size, but they create the smallest configuration, which will fulfill the SAP HANA KPIs.
Finally, a file system needs to be created on top of each volume group:

```
[root@hana256-vsi ~]# mkfs.xfs /dev/mapper/hana_vg-hana_log_lv
[root@hana256-vsi ~]# mkfs.xfs /dev/mapper/hana_vg-hana_data_lv
[root@hana256-vsi ~]# mkfs.xfs /dev/mapper/hana_vg-hana_shared_lv
```

The following entries to `/etc/fstab` will allow mounting the file systems after their mount points (`/hana/data`, `/hana/log` and  `/hana/shared`) have been created:

```
/dev/mapper/hana_vg-hana_log_lv    /hana/log xfs defaults,swalloc,nobarrier,inode64 0 0
/dev/mapper/hana_vg-hana_shared_lv /hana/shared xfs defaults,inode64 0 0
/dev/mapper/hana_vg-hana_data_lv   /hana/data xfs defaults,largeio,swalloc,inode64 0 0
```

### Storage for mx2-48x384 Profile-based VSIs

For a VSI deployment based on the **mx2-48x384** profile, slightly larger file systems are required. Again, three block storage volumes** of 500 GB size with a custom volume profile that supports up to 10,000 **Max IOPS** need to be attached to the VSI. In addition to that, four block storage volumes** of 100 GB size with a custom volume profile that supports up to 6,000 **Max IOPS** are required. In the example below, the 500 GB data volumes are `vdh` through `vdj`, the 100 GB volumes are `vdd` through `vdg`.

| Volume Group | Volume | Storage type | IOPS/GB | GB |
| --- | --- | --- | --- | --- |
| `hana_log_vg` | `vdd` | data volume | 6,000 | 100 GB
| `hana_log_vg` | `vde` | data volume | 6,000 | 100 GB
| `hana_log_vg` | `vdf` | data volume | 6,000 | 100 GB
| `hana_log_vg` | `vdg` | data volume | 6,000 | 100 GB
| `hana_vg` | `vdh` | data volume | 10,000 | 500 GB
| `hana_vg` | `vdi` | data volume | 10,000 | 500 GB
| `hana_vg` | `vdj` | data volume | 10,000 | 500 GB
{: caption="Table 3. Storage for mx2-48x384 profile based VSIs" caption-side="top"}

First, put the devices under LVM control by creating physical volumes:

```
[root@hana384-vsi ~]# pvcreate /dev/vd[d,e,f,g,h,i,j]
```

Then, two different volume groups need to be created:

```
[root@hana384-vsi ~]# vgcreate hana_vg /dev/vdh /dev/vdi /dev/vdj
[root@hana384-vsi ~]# vgcreate hana_log_vg /dev/vdd /dev/vde /dev/vdf /dev/vdg
```

Next, three logical volumes need to be defined on top. These logical volumes reflect the file system size requirements for SAP HANA. The following commands are for a 384 GB VSI:

```
[root@hana384-vsi ~]# lvcreate -l 100%VG -i 4 -I 64K  -n hana_log_lv hana_log_vg
[root@hana384-vsi ~]# lvcreate -i 3 -L 384G -I 64K -n hana_shared_lv hana_vg
[root@hana384-vsi ~]# lvcreate -i 3 -l 100%FREE  -I 64K -n hana_data_lv hana_vg
```

Finally, a file system needs to be created on top of each volume group:

```
[root@hana384-vsi ~]# mkfs.xfs /dev/mapper/hana_log_vg-hana_log_lv
[root@hana384-vsi ~]# mkfs.xfs /dev/mapper/hana_vg-hana_data_lv
[root@hana384-vsi ~]# mkfs.xfs /dev/mapper/hana_vg-hana_shared_lv
```

The following entries to `/etc/fstab` will allow mounting the file systems after their mount points (`/hana/data`, `/hana/log` and  `/hana/shared`) have been created:

```
/dev/mapper/hana_log_vg-hana_log_lv    /hana/log xfs defaults,swalloc,nobarrier,inode64 0 0
/dev/mapper/hana_vg-hana_shared_lv /hana/shared xfs defaults,inode64 0 0
/dev/mapper/hana_vg-hana_data_lv   /hana/data xfs defaults,largeio,swalloc,inode64 0 0
```

## High-availability configuration and support
{: #ha}

The {{site.data.keyword.cloud_notm}} environment does not support any pre-configured high availability (HA) scenarios. However, it does let you implement HA solutions for SAP HANA through Red Hat Enterprise Linux HA extensions, such as an on-premises case.

SAP HANA system replication can be configured with an automated fail-over from one server to a replica. Follow the SAP documentation on system replication to determine the replication mode that best fits your application scenario and your level of disaster resilience. Depending on the replication mode, different network KPIs need to be fulfilled. Consult SAP recommendations on the network throughput and latency to determine the required throughput and maximum latency for your operation mode of choice. The {{site.data.keyword.cloud_notm}} network topology should be able to serve all the required configurations. Contact {{site.data.keyword.cloud_notm}} Support to determine the optimal setup for your scenario if you're not sure or if you want your disaster recovery site in a different data center to achieve maximum disaster resilience.

Be aware that SAP HANA Scale-Out (multi node) environments are still under evaluation. In other words, a standby-node for SAP HANA is not a current option in an IBM VPC environment.

For more information on system replication, and network throughput and latency, see

  * [How To Perform System Replication for SAP HANA](https://www.sap.com/documents/2013/10/26c02b58-5a7c-0010-82c7-eda71af511fa.html){: external}
  * [Network Required for SAP HANA System Replication](https://www.sap.com/documents/2014/06/babb2b55-5a7c-0010-82c7-eda71af511fa.html){: external}

For more information on setting up the HA cluster extensions for your Linux operating system, see

  * [Automated SAP HANA System Replication with Pacemaker on RHEL Setup Guide](https://access.redhat.com/articles/1466063){: external}

### Network considerations
{: #network}

The network setup that is attached to a virtual server through the underlying hypervisor is highly available by its own design. However, further subnets need to be added to the virtual server nodes in a cluster to segregate cluster internal communication from other - client or application - communication. This setup can also be considered for DR scenarios that are implemented by the replication of a HANA database.

Depending on the network requirements, such as replication scenarios in a DR setup, you must check the location of the connected devices and any new network requirements that are specific to the product in question. Check with {{site.data.keyword.cloud_notm}} Support to determine which solution works best for your business process needs.

## Next steps
{: #determine-configuration-next-steps}

  * [Setting up your IBM Cloud account structure](/docs/sap-vpc-hana?topic=sap-vpc-hana-account-structure)
