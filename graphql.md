---

copyright:
  years: 2020
lastupdated: "2020-12-03"

keywords: graphql

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
{:table: .aria-labeledby="caption"}
{:external: target="_blank" .external}
{:table: .aria-labeledby="caption"}
{:generic: data-hd-programlang="generic"}
{:download: .download}
{:DomainName: data-hd-keyref="DomainName"}

# Setting up GraphQL
{: #graphql}

GraphQL is a query language for APIs. You can run queries on a single `/graphql` endpoint. GraphiQL uses a built-in introspection system to expose information through the schema.
{: shortdesc}

GraphQL analytics API endpoint is only available for Enterprise-level plans.
{: important}

## Prerequisites
{: #graphql-prereqs}

You must have the following permissions to proceed:
- You must have Viewer permission either at the instance level or zone level. 
- You must have Reader permission at the zone level.

### GraphiQL tool
{: #graphiql-tool}

Using GraphiQL, you can explore the schema and test queries for the GraphQL endpoint.
1. [Download and install GraphiQL](https://www.electronjs.org/apps/graphiql){: external}.
1. Open the GraphiQL application and enter the HTTP headers that you want to authorize.
1. Enter the correct endpoint in the **GraphQl Endpoint** field. Use `https://api.cis.cloud.ibm.com/v1/<crn>/zones/<zoneid>/graphql`.

The window is divided into a query pane and a response pane. You can define the query variables, which are interpolated into the query in the query pane. 

To build a query, follow these steps.

1. Select `POST` from the **Method** list menu.
1. Click **Edit HTTP Headers**.
1. Enter the `X-Auth-User-Token` or Authorization token, and set the Content-Type: `application/json`. Be sure to save them.

Start building your query in the query pane. Use the **Document Explorer** to explore the schema and documentation. The Document Explorer shows the different available options for datasets, dimensions, operations, and functions. The words "zone" and "domain" are synomyous in the Document Explorer.

Paste the following test snippet into the query pane and observe the response in the response pane, adjusting the `datetime` to meet your needs.

```
query { 
   viewer { 
      zones(filter: {zoneTag: "<your domain ID>"}) { 
         httpRequests1hGroups(limit: 5, filter: { datetime_gt: "2020-08-30T04:00:00Z", datetime_lt: "2020-08-31T06:00:00Z"}) { 
            sum { 
              countryMap { 
                bytes 
                clientCountryName 
              } 
            } 
            dimensions { 
              date 
              datetime 
             } 
          } 
         firewallEventsAdaptiveGroups(limit: 10, filter: { datetime_gt: "2020-08-30T04:00:00Z", datetime_lt: "2020-08-31T06:00:00Z"}) { 
               count 
               dimensions { 
                  clientCountryName 
                  clientAsn 
                  datetimeHour
               } 
             } 
          } 
       }
     }
```
{: codeblock}

## Querying basics
{: #querying-basics}

GraphQL structures data as a graph. To get the data you need, you can explore the edges of the graph. This is an example query format:

```
viewer {
      zones(filter:...) {
         requests(filter:...) {
           date, time, bytes,...
         }
      }
}
```
{: codeblock}

The initial node of the user running the query is `viewer`. A viewer is able to access one or more domains (zones). Each zone contains different datasets, such as firewall events for a zone.

Nodes that represent aggregated data include the `groups` suffix, for example, `firewallEventsAdaptiveGroups`. Each group follows a specific structure, shown in the following example. 

```
type ExampleGroup {
    count # No subfields, it is just the group size.
    sum {
        # fields that support summing (numbers, maps of numbers)
    }
    avg {
        # fields that support averaging (numbers)
    }
    uniq {
        # fields that support uniqueing (numbers, strings, enums, IPs, dates, etc.)
    }
}
```
{: codeblock}

This example shows a valid group:

```
  httpRequests1mGroups {
    sum {
      bytes
    }
    uniq {
      uniques # unique IPs
    }
    dimensions {
      datetimeMinute
    }
  }
```
{: codeblock}


## Datasets
{: #graphql-datasets}

The following datasets are available.

|Dataset | Node|
|--- | --- |
|Browser Insights | `browserInsightsAdaptiveGroups` |
|Firewall Activity Log | `firewallEventsAdaptive` `firewallEventsAdaptiveByTimeGroups` |
|Firewall Analytics | `firewallEventsAdaptiveGroups` |
|Health Check Analytics | `healthCheckEvents` `healthCheckEventsGroups` |
|HTTP Requests | `httpRequests1mGroups` `httpRequests1hGroups` `httpRequests1dGroups` `httpRequestsAdaptiveGroups` |
|Image Resizing Analytics | `imageResizingRequests1mGroups` |
|Load Balancing Analytics | `loadBalancingRequests` `loadBalancingRequestsGroups` |
|SYN Attacks (DoS Analytics) | `synAvgPps1mGroups` |
|Edge Functions Metrics | `workersInvocationsAdaptive` |


## Errors
{: #graphql-errors}

The GraphQL Analytics API is a RESTful API based on HTTPS requests and JSON responses and returns familiar HTTP status codes (for example, `404`, `500`, `504`). In conformity to the GraphQL specification, a `200` response can contain an error, which is in contrast to the common REST approach. 

All responses contain an errors array, which is null if there are no errors. Non-null errors contain `message`, `path`, and `timestamp`. 

The following code is an example error response:

```
{
  "data": null,
  "errors": [
    {
      "message": "cannot request data older than 2678400s",
      "path": [
        "viewer",
        "zones",
        "0",
        "firewallEventsAdaptiveGroups"
      ],
      "extensions": {
        "timestamp": "2019-12-09T21:27:19.195060142Z"
      }
    }
  ]
}
```
{: screen}


## Limits
{: #graphql-limits}

The limits for retaining historical data are defined in the following table.

|Data node | Enterprise|
|--- | --- |
|`browserInsightsAdaptiveGroups` | 30 days|
|`firewallEventsAdaptiveByTimeGroups` | 30 days|
|`firewallEventsAdaptiveGroups` | 30 days|
|`healthCheckEventsGroups` | 90 days|
|`healthCheckEvents` | 90 days|
|`httpRequestsAdaptiveGroups` | 30 days|
|`httpRequests1dGroups` | 365 days|
|`httpRequests1hGroups` | 90 days|
|`httpRequests1mGroups` | 7 days|
|`loadBalancingRequestsGroups` | 30 days|
|`loadBalancingRequests` | 30 days|
|`synAvgPps1mGroups` | 7 days|

### Query settings for account limits
{: #query-settings-account-limits}

To obtain specific information regarding limits for a data node, use the `settings` node.

Field | Description
--- | --- 
`enabled` | Returns `true` if the data set (node) is available for the current plan.
`maxDuration` | Defines the maximum time period (in seconds) that can be requested in one query (varies by data node).
`maxNumberOfFields` | Defines the maximum number of fields that can be requested in one query (varies by data node).
`maxPageSize` | Defines the maximum number of records that can be returned in one query (varies by data node).
`notOlderThan` | Limits how far back in the record a query can search (in seconds, varies by data node and plan).

**Example query**

```
{
  viewer {
    zones(filter: { zoneTag: $zoneTag }) {
      settings {
        browserInsightsAdaptiveGroups {
          maxDuration
          maxNumberOfFields
          maxPageSize
          enabled
          notOlderThan
        }
      }
    }
  }
}
```
{: codeblock}

**Response**

```
{
  "data": {
    "viewer": {
      "zones": [
        {
          "settings": {
            "browserInsightsAdaptiveGroups": {
              "enabled": true,
              "maxDuration": 2592000,
              "maxNumberOfFields": 30,
              "maxPageSize": 10000,
              "notOlderThan": 2595600
            }
          }
        }
      ]
    }
  },
  "errors": null
}
```
{: screen}

### Query limits
{:#query-limits}

The volume of data that a query can return is limited, and there are user limits on daily data volume. The following limits apply in addition to general rate limits enforced by the API:
* A zone-scoped query can include up to 10 zones.
* Queries can request up to 30 fields as indicated by `maxNumberOfFields` in `settings`.
* Responses can return up to 10,000 records. This limit is indicated by `maxPageSize` in `settings`. 

Queries must explicitly specify the upper bounds of records to return using the `limit` argument. 

## Sorting
{:#sorting}

You can sort the order of query result elements using the `orderBy` argument. By default, results are sorted by the primary key of the dataset (table). If you specify another field to sort on, the primary key is included in the sorting key to keep consistent results for pagination. 

Ordering within nested structures is not supported.
{: note}

### Sorting examples
{:#sorting-examples}

Raw data sorting:

```
firewallEventsAdaptive (orderBy: [clientCountryName_ASC]) {
    clientCountryName
}
```
{: codeblock}

Raw data sorting using multiple fields:

```
firewallEventsAdaptive (orderBy: [clientCountryName_ASC, datetime_DESC]) {
    clientCountryName
    datetime
}
```
{: codeblock}

Group sorting by aggregation function:

```
httpRequests1hGroups (orderBy: [sum_bytes_DESC]){
    sum {
        bytes
        requests
    }    
    dimensions {
        datetime
    }
}
```
{: codeblock}

## Pagination
{:#pagination}

Pagination, breaking up query results into smaller parts, can be done using `limit`, `orderBy`, and filtering paramters. The GraphQL Analytics API does not support cursors for pagination. 
* `limit` (integer) defines how many records to return.
* `orderBy` (string) defines the sort order for the data. 


### Query pages without cursors
{: #query-pages-without-cursors}

The following examples assume that `date` and `clientCountryName` relationships are unique.


### Get the first _n_ results of a query.
{: #query-get-first-n-results}

To limit results, add the `limit` parameter. For example, query the first two records.

```
firewallEventsAdaptive (limit: 2, orderBy: [datetime_ASC, clientCountryName_ASC]) {
    datetime
    clientCountryName
}
```
{: codeblock}

Specifying a sort order by date returns less specific results than specifying a sort order by both date and country.

**Response**

```
{
  "firewallEventsAdaptive" : [
    {
      "datetime": "2018-11-12T00:00:00Z",
      "clientCountryName": "UM"
    },
    {
      "datetime": "2018-11-12T00:00:00Z",
      "clientCountryName": "US"
    }
  ]
}
```
{: screen}

### Query for the next page using filter
{: query-next-page-filter}

To get the next _n_ results, specify a filter to exclude the last result from the previous query. Using the previous example, you can append the greater-than operator (`_gt`) to the `clientCountryName` field and the greater-or-equal operator  to the `datetime` field. By making a specific order, you can get the most complete results. 

```
firewallEventsAdaptive (limit: 2, orderBy: [datetime_ASC, clientCountryName_ASC], filter: {date_geq: "2018-11-12T00:00:00Z", clientCounterName_gt: "US"}) {
    date
    clientCountryName
}
```
{: codeblock}

**Response**

```
{
  "firewallEventsAdaptive" : [
    {
      "datetime": "2018-11-12T00:00:00Z",
      "clientCountryName": "UY"
    },
    {
      "datetime": "2018-11-12T00:00:00Z",
      "clientCountryName": "UZ"
    }
  ]
}
```
{: screen}

### Query the previous page
{: #query-previous-page}

To get the previous _n_ results, reverse the filters and sort order.

```
firewallEventsAdaptive (limit: 2, orderBy: [datetime_DESC, clientCountryName_DESC, filter: {date_leq: "2018-11-12T00:00:00Z", clientCountryName_lt: "UY"}]) {
  datetime
  clientCountryName
}
```
{: codeblock}

**Response**

```
{
  "firewallEventsAdaptive" : [
    {
      "datetime": "2018-11-12T00:00:00Z",
      "clientCountryName": "US"
    },
    {
      "datetime": "2018-11-12T00:00:00Z",
      "clientCountryName": "UM"
    }
  ]
}
```
{: screen}

## Filtering
{: #filtering}

Filters constrain queries to a particular account or set of zones (domains), requests by date, or a specific query agent. Without filters, performance might degrade, and results might include unimportant data. 

### Structure
{: #structure}

The GraphQL filter is represented by the [GraphQL Input Object](http://spec.graphql.org/June2018/#sec-Input-Objects){: external}.

Filters can be used as an argument on the following resources:
* Zones (Domains)
* Tables (data sets)
* Accounts

### Zone (domain) filter
{: #zone-filter}

The zone filter allows querying zone-related data by zone (domain) ID (`zoneTag`).

```
zones(filter: {zoneTag: "your domain (zone) ID"}) {
    ...
}
```
{: codeblock}

The zone filter must conform to the following grammar:

```
filter
    { zoneTag: t }
    { zoneTag_gt: t }
    { zoneTag_in: [t, ...] }
```
{: codeblock}

Compound filters (comma-separated, `AND`, `OR`) are not supported.
Zones always sort alphanumerically.

### Table (data set) filter
{: #table-filter}

Table filters require that you query at least one node. The `AND` operator can be used to create and combine multi-node filters.

### Accounts filter

Account level filtering is supported, and there is a required filter parameter. For example:

```
accounts(filter: {accountTag: $accountTag}) {
    ...
}
```
{: codeblock}

### Operators
Operator support varies, depending on the node type and node name. The following operators are supported for all types:

|Operator | Comparison|
|:---:|:---|
|`gt` | greater than|
|`lt` | less than|
|`geq` | greater or equal to|
|`leq` | less or equal to|
|`neq` | not equal|
|`in` | in|

### Examples
{:#graphql-examples}

General example:

```
{
  myQuery(
    filter: {
      clientCountry: "UK" # all objects having client country equal to "UK"
      datetime_gt: "2020-01-01 10:00:00" # all object having datetime greater than "2020-01-01 10:00:00"
    }
  )
}
```
{: codeblock}

Filter a specific node:

```
httpRequests1hGroups(filter: {datetime: "2020-01-01 10:00:00"}) {
    ...
}
```
{: codeblock}

Filter on multiple fields:

```
httpRequests1hGroups(filter: {datetime_gt: "2018-01-01 10:00:00", datetime_lt: "2018-01-01 11:00:00"}) {
    ...
}
```
{: codeblock}

Filter using `OR` operator:

```
httpRequests1hGroups(filter: {
    datetime: "2020-01-01 10:00:00",
    OR:[{clientCountryName: "US"}, {clientCountryName: "UK"}]) {
    ...
}
```
{: codeblock}
