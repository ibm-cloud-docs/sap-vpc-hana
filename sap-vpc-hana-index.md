---

copyright:
  years: 2020
lastupdated: "2020-09-02"

keywords: SAP in VPC, {{site.data.keyword.cloud_notm}}, {{site.data.keyword.cos_full_notm}}, {{site.data.keyword.cos_short}}, {{site.data.keyword.openshiftlong_notm}}, {{site.data.keyword.openshiftshort}}, Red Hat Enterprise Linux, SAP Data Hub on {{site.data.keyword.cloud_notm}}, data orchestration, data governance, data integration

subcollection: sap-vpc

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:external: target="_blank" .external}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:note: .note}

# Getting started with SAP HANA on IBM Cloud Virtual Private Cloud
{: #getting-started}

{{site.data.keyword.IBM}} and SAP continue a 45-year+ collaboration in multiple areas, including hardware, software, cloud, services, and finance. They are now collaborating to optimize the {{site.data.keyword.vpc_full}} (VPC) infrastructure products to include support for the SAP HANA solution in the more than 60 IBM Cloud data centers worldwide.
{: shortdesc}

This content provides you with recommendations for the provisioning and installation of the SAP HANA infrastructure and subscription on {{site.data.keyword.vpc_short}}. It does not replace any SAP HANA documentation nor is it intended to give a full description of the installation process. Its purpose is to help you with infrastructure planning and provisioning so you can begin your SAP installation. The SAP installation (including database installation) does not vary from installations for on-premises environments. Recommendations and guidelines are provided to help you operate your SAP system in the {{site.data.keyword.vpc_short}} environment regarding the infrastructure provided, for example, network setup or back up storage. The operation of the SAP HANA system does not differ from its operation in any other data center after the infrastructure is in place.


## Before you begin
{: #before-you-begin-getting-started}

Before you can install SAP HANA on {{site.data.keyword.vpc_short}}, you need an SAP S-User ID and an IBMid to review relevant SAP and {{site.data.keyword.cloud_notm}} documentation. For more information about getting an S-User ID or an IBMid, see [Getting SAP and IBM Cloud credentials and create accounts](/docs/sap-vpc-hana?topic=sap-vpc-hana-planning-your-system-landscape#get_sap_ibm_credentials). The following is {{site.data.keyword.cloud_notm}} and SAP content to review before implementing SAP HANA on {{site.data.keyword.vpc_short}}.

  * [What is the IBM Cloud platform?](/docs/overview?topic=overview-whatis-platform)
  * [SAP Notes through SAP Support](https://support.sap.com/en/index.html){: external}
  * [{{site.data.keyword.vpc_short}} Overview](/docs/vpc?topic=vpc-about-vpc) (through Next Steps)

## Building your infrastructure
{: #build-infrastructure}

Use the following table to help you get your {{site.data.keyword.vpc_short}} infrastructure built quickly.

| Task | Details |
|--- | --- |
| 1. Access the IBM Cloud console | [IBM Cloud console](https://cloud.ibm.com){: external} is your graphical gateway to all your infrastructure components - compute, connectivity, storage, network, and data centers. You need an IBMid and password to access the console. When you first log in to the console, the default page, Dashboard, shows the existing resources that are in your IBM Cloud account. |
| 2. Plan your system landscape | Use the information in [Planning your system landscape](/docs/sap-vpc-hana?topic=sap-vpc-hana-planning-your-system-landscape) to architect, size, and provision your {{site.data.keyword.vpc_short}} environment to support your SAP HANA workload. |
| 3. Set up your IBM Cloud account infrastructure | Follow the steps and information in [Setting up your IBM Cloud account structure](/docs/sap-vpc-hana?topic=sap-vpc-hana-account-structure) to set up your IBM Cloud account infrastructure to support your SAP HANA workload. |
| 4. Set up your infrastructure | Use the steps in [Setting up your infrastructure](/docs/sap-vpc-hana?topic=sap-vpc-hana-set_up_infrastructure-vpc) to order a virtual server, order VPC Block Storage, and more. |
| 5. Install SAP software | Install SAP software on your IBM Cloud infrastructure the same as if the servers were on-premises. For more information about installing SAP software, see [Downloading and installing SAP software and applications](/docs/sap-vpc-hana?topic=sap-vpc-hana-install_sap). |
{: caption="Table 1. Steps to get up and running quickly" caption-side="top"}
