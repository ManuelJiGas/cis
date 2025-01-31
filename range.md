---

copyright:
  years: 2018, 2020
lastupdated: "2020-07-06"

keywords: range application, tls encryption, ddos protection, global tcp proxy

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

# Protecting TCP traffic (Range)
{: #cis-range}

The Range feature brings DDoS protection, load balancing, and content acceleration to any TCP-based protocol.
Range is a global TCP proxy running on {{site.data.keyword.cis_full}} (Cloudflare) edge nodes.
{: shortdesc}

Range can be used to:
* Protect your TCP ports and protocols from Layer-3 and Layer-4 DDoS attacks.
* Protect your HTTP(S) Range app with Layer-7 [firewall rules](/docs/cis?topic=cis-firewall-rules).
* Reduce the ability of attackers to snoop and steal sensitive data by enabling TLS encryption.
* Integrate with CIS IP firewall, which allows you to block or challenge IP addresses, or entire IP ranges, from reaching your TCP services.
* Configure load balancers with TCP health checks, failover, and steering policies to dictate where traffic flows.

## Getting started with Range
{: #getting-started-with-range}

Range is only available to Enterprise customers for an additional cost, and is priced per bandwidth usage.
{: note}

### Adding an application
{: #range-add-an-application}

Follow these steps to add an application.

1. Navigate to **Security > Range**.
1. Click **Add application**.
1. Enter the application name in the first input field. Your application becomes associated with a DNS name on your {{site.data.keyword.cis_short_notm}} domain.
1. Enter the edge port in the next input field. We'll listen for incoming connections to these addresses on this port. Connections to these addresses are proxied to your origin. (Proxying is supported on all ports except port 21.)
1. In the Origin section, enter the origin IP and port of your TCP application. You can also select an existing load balancer.
1. Enable IP firewall. When enabled, firewall rules with a "block" or "allowlist" action are enforced for this application.
1. Enable PROXY Protocol if you have a proxy in-line that supports PROXY Protocol v1. This feature is useful if you are running a service that requires knowledge of the true client IP. In most cases, this setting remains disabled. 
1. Click **Provision**.

Proxy protocol prepends every connection with a header reporting the client IP address and port. A Proxy Protocol header has the format: 

PROXY_STRING + single space + INET_PROTOCOL + single space + CLIENT_IP + single space + PROXY_IP + single space + CLIENT_PORT + single space + PROXY_PORT + "\r\n"

* Here's an example PROXY Protocol line for an IPv4 address:
    `PROXY TCP4 192.0.2.0 192.0.2.255 42300 443\r\n`
    
* Here's an example PROXY Protocol line for an IPv6 address:
    `PROXY TCP6 2001:db8:: 2001:db8:ffff:ffff:ffff:ffff:ffff:ffff 42300 443\r\n`

Provisioning a Range application incurs additional costs, based on the amount of bandwidth used per app.
{: note}

Your application is now visible in a tile with the following properties:

* Application name
* Edge port
* Origin & port
* Connections from the past hour (polled every minute)
* Throughput from the past hour (polled every minute)
* Overflow menu (top right corner) allows the following:
    * Edit the application
    * View metrics for the specified application
    * Delete the application

When a Range application is created, it is assigned a unique IPv4 and IPv6 address. These IP addresses are not static and might be subject to change. You can determine the assigned IP address by using DNS. The DNS name always returns the IP addressed assigned to the application.     

### Range limitations
{: #range-limitations}

You can create a maximum of 10 Range applications with unique origins. Each Range application with a unique origin must have a unique IP address allocated, and IP addresses are a limited resource. If you need more than 10 Range applications, open an IBM Support ticket. Support tickets to add more Range applications require a review of the use case, and the process can take a few days.

You can create more than 10 applications if they reuse an existing origin, but use different ports.
{: tip}

For TCP Range apps, only IP rules apply. This is because IP rules are applied to OSI Layer 3 and Layer 4. However HTTP(S) Range apps work with both firewall rules and IP rules. In general, firewall rules are designed for properties exposed in OSI Layer 7 (HTTP), such as request headers and body content characteristics.

### Viewing metrics
{: #range-view-metrics}

Your application is now ready to proxy TCP traffic through {{site.data.keyword.cis_short_notm}} (Cloudflare).

Navigate to **Metrics > Range** to view your number of connections to applications and throughput traffic.
The graphs show metrics for up to 10 applications.
{: note}

To toggle application metrics, use the Chart key or click the **Select applications** button. To change the Metrics data time frame, use the list menu.

## Range AppTiles
{: #range-apptiles}

After creating a few apps, the **Security > Range** page is populated with applications tiles. The application tiles contain the following information:

* Application name
* Edge port
* Origin & port
* Connections from the past hour (polled every minute)
* Throughput from the past hour (polled every minute)

The application tile also contains an overflow menu in the top corner. The overflow menu provides users the option to:

* Edit the application
* View metrics for the specified application
    * This takes the user to **Metrics > Range** page, which displays the metrics for only that application.
* Delete the application

## API usage examples
{: #range-api-usage-examples}

These are examples to create and list applications using Range.

### Creating a Range app
{: #create-range-app}

There are two ways you can designate an origin in a Range app.

1. Origin IP - use parameter `origin_direct`
2. Load balancer - use parameters `origin_dns` and `origin_port`

**Request:**

```
curl -X POST \
  https://api.cis.cloud.ibm.com/v1/<url-encoded-crn>/zones/<zone-d>/range/apps \
  -H 'X-Auth-User-Token: <token>' \
  -d '{"protocol":"tcp/22","dns":{"type":"CNAME","name":"ssh.example.com"},"origin_direct":["tcp://172.0.2.1:22"],"proxy_protocol":true,"ip_firewall":true}'

```
{: codeblock}

**Response:**

```
{
    "result": {
        "id": "4f70c3d4f20576b79135b898295e8093",
        "protocol": "tcp/22",
        "dns": {
            "type": "CNAME",
            "name": "ssh.example.com"
        },
        "origin_direct": [
            "tcp://172.0.2.1:22"
        ],
        "ip_firewall": true,
        "proxy_protocol": true,
        "created_on": "2019-01-09T17:33:09.190606Z",
        "modified_on": "2019-01-09T17:33:09.190606Z"
    },
    "success": true,
    "errors": [],
    "messages": []
}
```
{: codeblock}

**Request:**

```
curl -X POST \
  https://api.cis.cloud.ibm.com/v1/<url-encoded-crn>/zones/<zone-d>/range/apps \
  -H 'X-Auth-User-Token: <token>' \
  -d '{"protocol":"tcp/22","dns":{"type":"CNAME","name":"ssh.example.com"},"origin_dns": {"name": "test"},"proxy_protocol":true,"ip_firewall":true, "origin_port": 43}'
```
{: codeblock}

**Response:**

```
{
    "result": {
        "id": "4f70c3d4f20576b79135b898295e8093",
        "protocol": "tcp/22",
        "dns": {
            "type": "CNAME",
            "name": "ssh.example.com"
        },
        "origin_dns": {
          "name": "test"
        },
        "origin_port": 43,
        "ip_firewall": true,
        "proxy_protocol": true,
        "created_on": "2019-01-09T17:39:09.190606Z",
        "modified_on": "2019-01-09T17:39:09.190606Z"
    },
    "success": true,
    "errors": [],
    "messages": []
}
```
{: codeblock}

* **DNS Name** - Your application is associated with a DNS name on your domain.
* **Protocol/Edge port** - Port on which your application is running. Connections to these addresses are proxied to your origin.
* **Origin direct** - IP on which your application is running, and the port that you want traffic to flow through from the edge to your origin.
* **IP Firewall** - If enabled, firewall rules with a Block action are enforced for this Range application.
* **PROXY Protocol** - Enable if you have a proxy in-line that supports PROXY Protocol v1. In most cases, this setting remains disabled.
* **Origin DNS** - Name of the load balancer that you want to set as your origin.
* **Origin Port:** - Port of your service.

### Listing all Range apps
{: #range-list-all-apps}

**Request:**

```
curl -X GET \
  https://api.cis.cloud.ibm.com/v1/<url-encoded-crn>/zones/<zone-d>/range/apps
```
{: pre}

**Response:**

```
{
    "result": [
        {
            "id": "4f70c3d4f20546b79135b898295e8093",
            "protocol": "tcp/22",
            "dns": {
                "type": "CNAME",
                "name": "ssh.example.com"
            },
            "origin_direct": [
                "tcp://172.0.2.1:22"
            ],
            "ip_firewall": true,
            "proxy_protocol": true,
            "created_on": "2019-01-09T17:33:09.190606Z",
            "modified_on": "2019-01-09T17:33:09.190606Z"
        }
    ],
    "success": true,
    "errors": [],
    "messages": []
}
```

### Listing a specfic Range app
{: #range-list-a-specific-range-app}

**Request:**

```
curl -X GET \
  https://api.cis.cloud.ibm.com/v1/<url-encoded-crn>/zones/<zone-d>/range/apps/4f70c3d4f20546b79135b898295e8093
```
{: pre}

**Response:**

* App using the Origin IP

    ```
    {
        "result": {
            "id": "4f70c3d4f20546b79135b898295e8093",
            "protocol": "tcp/22",
            "dns": {
                "type": "CNAME",
                "name": "ssh.example.com"
            },
            "origin_direct": [
                "tcp://172.0.2.1:22"
            ],
            "ip_firewall": true,
            "proxy_protocol": true,
            "created_on": "2019-01-09T17:33:09.190606Z",
            "modified_on": "2019-01-09T17:33:09.190606Z"
        },
        "success": true,
        "errors": [],
        "messages": []
    }
    ```
    {: codeblock}

* App using a load balancer

    ```
    {
        "result": {
            "id": "555359036e7f4acc82d69b916f62caba",
            "protocol": "tcp/22",
            "dns": {
                "type": "CNAME",
                "name": "ssh.example.com"
            },
            "origin_dns": {
                "name": "test_update"
            },
            "origin_port": 76,
            "ip_firewall": true,
            "proxy_protocol": true,
            "created_on": "2019-01-10T22:26:47.167008Z",
            "modified_on": "2019-01-10T22:26:47.167008Z"
        },
        "success": true,
        "errors": [],
        "messages": []
    }
    ```
    {: codeblock}
