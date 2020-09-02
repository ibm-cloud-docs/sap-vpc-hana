---

copyright:
  years: 2020
lastupdated: "2020-09-02"

keywords: SAP NetWeaver, {{site.data.keyword.cloud_notm}}, deployment, BYOL, database

subcollection: sap-vpc-hana

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:external: target="_blank" .external}
{:pre: .pre}
{:note: .note}
{:important: .important}
{:table: .aria-labeledby="caption"}
{:note: .note}

# Setting up your infrastructure
{: #set_up_infrastructure-vpc}

**Setting up your infrastructure** provides guidance on how to set up SAP HANA on {{site.data.keyword.vsi_is_full}}. Instructions include setting up your network, security, storage, and operating system (OS). For more information about {{site.data.keyword.vsi_is_short}}, see [About virtual server instances for VPC](/docs/vpc?topic=vpc-about-advanced-virtual-servers).
{:shortdesc}

## Before you begin
{: #before-you-set-up-infrastructure-vpc}

Before you can create a virtual server instance, you must create a VPC as described in [Setting up your {{site.data.keyword.cloud_notm}} account structure](/docs/sap-vpc-hana?topic=sap-vpc-hana-account-structure) and detailed further in the [Create an {{site.data.keyword.vpc_short}}](/docs/vpc?topic=vpc-creating-a-vpc-using-the-ibm-cloud-console) section.
In addition, you must create an **SSH key** that you add to the server instance during its creation. See more details here: [SSH keys](/docs/vpc?topic=vpc-ssh-keys).
{:important}

## Ordering your virtual server
{: order-virtual-server-vpc}

Use the following steps to order your virtual server and necessary components. For more information about creating a virtual server, see [Creating virtual server instances by using the IBM Cloud console](/docs/vpc?topic=vpc-creating-virtual-servers).

1. Log in to the [{{site.data.keyword.cloud_notm}} console](https://cloud.ibm.com){: external} with your unique credentials.
1. Click **Menu icon ![Menu icon](../../icons/icon_hamburger.svg) > VPC Infrastructure > Virtual server instances**.
1. Click **New instance**.
1. Enter a unique **Name** for the virtual server, which becomes the **hostname**. SAP hostnames must consist of a maximum of 13 alpha-numeric characters. For more information about SAP hostnames, see [SAP Notes 611361](https://launchpad.support.sap.com/#/notes/611361){: external} and [129997](https://launchpad.support.sap.com/#/notes/129997){: external}.
1. Select the **Virtual private cloud** in which to attach the virtual server.
1. Choose a **Resource group**.

   The resource group can't be changed after the virtual server is created.
   {: note}

1. Select the same **Location** in which you created your subnets. The location consists of a region and zone.
1. Select **Catalog Image** > **`ibm-redhat-7-6-amd64-sap-hana-1`** as the OS image.
1. Select a **Profile** based on the guidance detailed in [Choosing an SAP-certified virtual server](/docs/sap-vpc-hana?topic=sap-vpc-hana-size_the_server#choose_server), which lists the profiles that are certified for SAP HANA.
1. Select the **SSH key** you want to add to the virtual server. For this step, you can create a new SSH key.
1. Click **New volume** for **Data volumes**. Data volumes are required and must meet special KPI needs that are defined by SAP and are mandatory. See [Storage configuration](/docs/sap-vpc-hana?topic=sap-vpc-hana-determine_configuration#storage_config) to learn these special needs and how you should configure the data volumes. You can enable **Auto Delete** to automatically delete the data volume if the virtual server is deleted. However, this is not recommended. Attach the appropriate data volumes to your virtual server.
1. Let **Network interfaces** default.
1. Click **Create virtual server instance**. Once the virtual server is provisioned and ready for SSH logon, you can begin installing SAP HANA applications.


Table 1 is a summary of the fields and values that are used to provision {{site.data.keyword.vsi_is_short}}.

| Field | Value |
|-------|-------|
| Name  | Unique name for your virtual server. |
| Virtual private cloud | Specify the VPC where you want to create your virtual server. |
| Resource group | Use resource groups to organize your account resources for access control and billing purposes. |
| Location | Locations are composed of regions (specific geographic areas) and zones (fault tolerant data centers within a region). Select the location where you want to create your virtual server. |
| Image | Select Catalog images > **`ibm-redhat-7-6-amd64-sap-hana-1`** for SAP HANA workloads. |
| Profile |  Select one of the profiles outlined in [Choosing an IBM Cloud for Virtual Servers for Virtual Private Cloud](/docs/sap-vpc-hana?topic=sap-vpc-hana-size_the_server#choose_server). |
| SSH Key | You must select an existing SSH key or upload a new SSH key before you can create the instance. SSH keys are used to securely connect to a running instance. |
| | **Note:** Alpha-numeric combinations are limited to 100 characters. For more information, see [SSH keys](/docs/vpc?topic=vpc-ssh-keys). |
| User data | You can add user data to automatically perform common configuration tasks or run scripts. For more information, see [User data](/docs/vpc?topic=vpc-user-data). |
| Boot volume | The default boot volume size for all profiles is 100 GB. You can edit the boot volume by clicking the pencil icon. You can add one or more secondary data volumes when you provision the instance. To add volumes, click **New volume**. For more information about provisioning the volume, see [Create and attach a block storage volume when you create a new instance](/docs/vpc?topic=vpc-creating-block-storage#create-from-vsi). |
| Data volume  | Attach data volumes to your virtual server depending on your SAP HANA virtual server profile. |
| Network interfaces | Assign networking options to connect into the IBM Cloud VPC. You can create and assign up to five network interfaces to each instance. |
{: caption="Table 1. Instance provisioning selections" caption-side="top"}


## Adding {{site.data.keyword.block_storage_is_short}}
{: #adding-vpc-block-storage-vpc}

1. Log in to the [{{site.data.keyword.cloud_notm}} console](https://cloud.ibm.com){: external} with your unique credentials.
1. Click **Menu icon ![Menu icon](../../icons/icon_hamburger.svg)** > **VPC Infrastructure** > **Block storage volumes** > **New Volume**.
1. Enter a unique **Name**.
1. Choose a **Resource group**.

   The resource group can't be changed after the virtual server has been created.
   {:important}

1. Select the same **Location** in which you created your {{site.data.keyword.vsi_is_short}}. The location consists of a region and zone.
1. Select either **IOPS tiers** or **Custom**. As described in [Items to consider when you plan your SAP landscape](/docs/sap-vpc-hana?topic=sap-vpc-hana-considerations), the IOPS per GB depends on the workload anticipated.
1. Click **Create volume**.

   By default all block storage for VPC volumes use IBM-managed encryption. However, [new capabilities are available for customer-managed encryption](/docs/vpc?topic=vpc-block-storage-vpc-encryption).
   {:note}

1. Click **Menu icon ![Menu icon](../../icons/icon_hamburger.svg)** > **VPC Infrastructure** > **Virtual server instances**.
1. Select the virtual server to which the new block storage will be attached.
1. Click **Attach volume**.
1. Select the new block storage volume and click **Attach**.
1. Log in to the virtual server over SSH, and perform the necessary activities for the block storage, such as add file system partitions, file systems, and mount points.

   See here more information about [Storage configuration for SAP HANA virtual servers](/docs/sap-vpc-hana?topic=sap-vpc-hana-determine_configuration#storage_config).
   {:note}

## Bring your own OS product license
{: #os-byol-vpc}

When you have your own operating system license, you can install it on your virtual server based on the vendor's instructions. For more information about custom images, see [Importing and managing custom images](/docs/vpc?topic=vpc-managing-images).

## Next Steps
{: #setting-up-infrastructure-next-steps-vpc}

  * [Accessing your infrastructure](/docs/sap-vpc-hana?topic=sap-vpc-hana-access-infrastructure)
