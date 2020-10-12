---

copyright:
  years: 2020
lastupdated: "2020-09-02"

keywords: SAP HANA, downloading SAP software, installing SAP software, SAP Download Manager, SAP Certified

subcollection: sap-vpc-hana

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:external: target="_blank" .external}
{:pre: .pre}
{:table: .aria-labeledby="caption"}

# Downloading and installing SAP software and applications
{: #install_sap}

Use the following steps to download and install SAP software and applications.

- Log in to the [SAP Support Portal](https://support.sap.com/en/index.html){: external} and click **Download Software** to download the required SAP software installation media. You can download to a local machine and upload it later to an IBM Cloud storage option (such as IBM Cloud Object Storage) or directly to a virtual server.

- After you connect to the SAP Marketplace, you can choose the SAP software components to download. You can either download individually by using a web browser; or add the required software to the [**SAP Download Basket**](https://launchpad.support.sap.com/#/downloadbasket){: external} on the SAP Support Portal and use the [SAP Download Manager](https://support.sap.com/en/my-support/software-downloads.html#section_995042677){: external}.

- To patch your virtual server instance operating system to the latest level, run `yum update` (RHEL) or `zypper update` (SLES) and restart the virtual server instance.

- To prevent the {{site.data.keyword.cloud_notm}} `cloudinit` process from reverting back the content of **`/etc/hosts`** to the previous values on the next restart, change the configuration in `/etc/cloud/cloud.cfg`. Change `manage_etc_host` from `True` to `False`.

- After the SAP software installation media downloads, follow the standard SAP installation procedure that is documented in the applicable [SAP installation guide](https://service.sap.com/instguides){: external} for your SAP version and components and the corresponding [SAP Notes](https://support.sap.com/en/my-support/knowledge-base.html){: external}. The installation guide and SAP Notes require an SAP S-user ID to view.
