---
title: Best Practices | OANDA API
---


# REST API Best Practices


* TOC
{:toc}

----------------------------

## Introduction

The OANDA API development team strives to bring the best overall experience for our API users. Here is a set of best practices to use the API as efficiently as possible.

----------------------------

## HTTP Persistent Connection

This keeps the underlying TCP connection active for multiple requests/responses. Subsequent requests will result in reduced latency as the TCP handshaking process is no longer required.

If you are using an HTTP 1.0 client, ensure it supports the keep-alive directive and submit the header `Connection: Keep-Alive` with your request.

Keep-Alive is part of the protocol in HTTP 1.1 and enabled by default on compliant clients. However, you will have to ensure your implementation does not set the connection header to other values.


|Action | Baseline (No Keep Alive) | Keep Alive Enabled | Latency Decrease |
|:---|:---:|:---:|:---:|
|Create Trade|341.7ms|117.8ms|65.5%|
|Closing Trade|340.3ms|118.0ms|65.3%|
|Get (100) Trades|399.8ms|236.9ms|40.7%|
|Get (500) Trades|559.5ms|434.5ms|22.3%|

<p class="best-practice-footnote">Conducted with test client located in Ireland.<br/>
Values are based off of the median latencies of 50 consecutive requests.</p>

----------------------------

## Compression

We recommend that compression be enabled for all REST API requests.

Internal testing have shown that for certain requests, the compressed response was 1/10th the size of the uncompressed response.   Tests have also shown reduced latencies as a result of the smaller response size.

To enable compression for responses, submit your request with the HTTP header and value `Accept-Encoding: gzip, deflate`.


|Action|Compression Disabled|Compression Enabled|Latency Decrease|
|:---|:---:|:---:|:---:|
|Get (50) Trades|167.6ms|66.7ms|60.2%|
|Get (100) Trades|232.1ms|127.9ms|44.9%|
|Get (500) Trades|339.7ms|157.1ms|53.8%|

<p class="best-practice-footnote">Conducted with test client located in USA (Northern California). <br/> 
Values are based off of the median latencies of 50 consecutive requests.<br/>
All requests had Keep Alive enabled.</p>

----------------------------

##ETag

If your application makes repeated calls of the same request, we highly recommend that you utilize ETag in your requests as it can greatly reduce response size.  See [here](/docs/v1/guide/#etag) for more information and usage.

Note: ETags can not be used in conjunction with compressed responses.

----------------------------

##Transactions

####Recent Transactions 

By default, the /transactions endpoint returns the most recent 50 transactions.  Up to 500 recent transactions may be returned by specifying the count parameter in the request.  However, this type of request is rate limited to 1 request per minute.

If your application requires quick access to recent transactions, it is recommended that your application make frequent requests to the default /transactions endpoint and cache the data on your local system.  To ensure there is no overlap in transaction responses, subsequent requests should set the minId parameter to the value of the most recent transaction ID returned.

####Full Transaction History

The [/alltransactions](/docs/v1/transactions/#get-full-account-history) endpoint allows users to obtain transaction records in a compressed json encoded file.  This file will include all transactions that range from the time of account creation to 4pm of the last trading day. There is a noticeable wait period between when the request is made and when the file is available for download. As such, it is recommended that this endpoint be only used to fill gaps in transaction history.

----------------------------

## Streaming

#### Session Id

This is only applicable to HTTP rates streaming.

The HTTP [rates stream](/docs/v1/stream/#rates-streaming) supports multiple connections with the same access token. This allow users to have a redundant rate stream by employing multiple clients connecting from different geographical locations.  We recommend using session ids to uniquely identify each connection so that reconnection of a particular connection does not affect other existing connections. An example of such usage can be found below.

This example uses two separate clients using the same access token, each with their own rates stream:

* Client 1 establishes a rates stream using "sessionId=Client1" in the request, and begins receiving rates.

* Client 2 establishes a rates stream using "sessionId=Client2" in the request, and begins receiving rates.

* An error occurs for Client 2.

* Client 2 needs to re-establish a rates stream and does so by specifying "sessionId=Client2" in the request.

* The existing rates stream connected to Client 2 is now disconnected and a new connection is established to Client 2.

* Through all of this, Client 1 is unaffected.

__Note__: If no session id were specified, the oldest connection would have been disconnected (i.e. Client 1).

----------------------------

## X-HTTP-Method-Override

There are HTTP clients (such as Adobe Flash/Flex) that do not support PATCH or DELETE requests.  To workaround this, please look into using the
[X-HTTP-Method-Override](/v1/guide/#x-http-method-override) HTTP header.

