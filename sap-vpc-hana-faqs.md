---

copyright:
  years: 2020
lastupdated: "2020-09-02"

keywords: SAP NetWeaver, {{site.data.keyword.cloud_notm}}, {{site.data.keyword.Db2_on_Cloud_short}}, FAQs, subnet, application server, SAP Certified, high availability, highly available

subcollection: sap-vpc

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:external: target="_blank" .external}
{:faq: data-hd-content-type='faq'}
{:pre: .pre}
{:table: .aria-labeledby="caption"}

# FAQs: SAP on IBM Virtual Private Cloud
{: #faqs}

## Can I split my distributed SAP environment between different data centers?
{: faq}

For a distributed SAP installation, it is best to have all nodes in the same data center and subnet. Deviation from this setup can yield latency and timeouts, which render your SAP system unresponsive.

## Can I achieve SAP high availability as defined by SAP architecture?
{: faq}

The only supported high availability architecture is the scaling of application servers. No enabled hardware is available for high availability support.

## Can I download the SAP distribution software directly from {{site.data.keyword.vpc_short}}?
{: faq}

We don't have a direct link for SAP software downloads. You need to download the distribution DVDs.
