---
id: proxy
title: Proxy
---

Overview of the Elrond Proxy

## **Architectural Overview**

While any Node in the Network can accept Transaction requests, the Transactions are usually submitted to the **Proxy** application, which maintains a list of Nodes - **Observers** - to forward Transaction requests to - these Observers are selected in such manner that any Transaction submitted to them will be processed by the Network **as soon and as efficiently as possible**.

The Proxy will submit a Transaction on behalf of the user to the REST API of one of its listed Observers, selected for **(a)** being _online_ at the moment and **(b)** being located **within the Shard to which the Sender's Account belongs**. After receiving the Transaction on its REST API, that specific Observer will propagate the Transaction throughout the Network, which will lead to its execution.

The Observer Nodes of the Proxy thus act as a **default dedicated entry point into the Network**.

It is worth repeating here, though, that submitting a Transaction through the Proxy is completely optional - any Node of the Network will accept Transactions to propagate, given it has not disabled its REST API.

![img](https://gblobscdn.gitbook.com/assets%2F-LhHlNldCYgbyqXEGXUS%2F-M93nKd9VLSYHsguW1PH%2F-M93o5wmBtDFoPSwcwmw%2FElrond%20Proxy%20-%20Purpose%20(overview).png?alt=media&token=55699234-8846-407d-aa16-a0646f6c3748)

Overview of the Elrond Proxy

In the figure above:

1. The **Elrond Network** - consisting of Nodes grouped within Shards. Some of these Nodes are **Observers**.
2. One or more instances of the **Elrond Proxy** - including the official one - connect to Observer Nodes in order to forward incoming user Transactions to the Network and to query state within the Blockchain.
3. The **client applications** connect to the Network through the Elrond Proxy. It is also possible for a blockchain-powered application to talk directly to an Observer or even to a Validator.

## **Official Elrond Proxy**

The official instance of the Elrond Proxy is located at [https://gateway.elrond.com](https://gateway.elrond.com/).

## **Set up a Proxy Instance**

:::warning
Documentation for setting up a Proxy is preliminary and subject to change
:::

In order to host a Proxy instance on a web server, one has to first clone and build the repository:

```
git clone https://github.com/ElrondNetwork/elrond-proxy-go.git
cd elrond-proxy-go/cmd/proxy
go build .
```

### **Configuration**

The Proxy holds its configuration within the `config` folder:

- `config.toml` - this is the main configuration file. It has to be adjusted so that the Proxy points to a list of chosen Observer Nodes.
- `external.toml` - this file holds configuration necessary to Proxy components that interact with external systems. An example of such an external system is **Elasticsearch** - currently, Elrond Proxy requires an Elasticsearch instance to implement some of its functionality.
- `apiConfig/credentials.toml` - this file holds the configuration needed for enabling secured endpoints - only accessible by using BasicAuth.
- `apiConfig/v1_0.toml` - this file contains all the endpoints with their settings (open, secured and rate limit).

## **Dependency on Elasticsearch**

Currently, Proxy uses the dependency to Elasticsearch in order to satisfy the [Get Address Transactions](/sdk-and-tools/rest-api/addresses/#get-address-transactions) endpoint. 

In order to connect a Proxy instance to an Elasticsearch cluster, one must update the `external.toml` file.
