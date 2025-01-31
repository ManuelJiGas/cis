---

copyright:
  years: 2018, 2020
lastupdated: "2020-06-23"

keywords: whitelisted IP addresses, CIS Edges, allowlisted IP addresses

subcollection: cis

---
{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:term: .term}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:deprecated: .deprecated}
{:external: target="_blank" .external}
{:generic: data-hd-programlang="generic"}
{:download: .download}
{:DomainName: data-hd-keyref="DomainName"}

# {{site.data.keyword.cis_short_notm}} allowlisted IP addresses
{: #cis-allowlisted-ip-addresses}

The following API lists all IP addresses used by the CIS proxy. The CIS proxy uses only addresses from this list, for both client-to-proxy and proxy-to-origin communication.

[https://api.cis.cloud.ibm.com/v1/ips](https://api.cis.cloud.ibm.com/v1/ips)

Polling this API one time a week is sufficient to get the information you need to update your allowlists. 

The IP addresses the CIS proxy uses for communication with the origins are not necessarily the same as the ones used for client-to-proxy communication, although all addresses are derived from the same list.
{: note}
