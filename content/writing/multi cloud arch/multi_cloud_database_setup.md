---
title: Database setup
tags:
  - Software
  - Architecture
  - developer
  - multi-cloud
  - hybrid-cloud
  - type/write
---

The most important part in every project is the database. It's where all your data lives and it's what makes your application run smoothly. In this article, we'll explore how to set up a database for your multi-cloud architecture.

## 1. Choose a Database

There are many different types of databases available, each with its own strengths and weaknesses.

However we focused to distributed database solutions in which can run on multi datacenters, with the common intefaces/drivers for almost-common frameworks to interact with. So we will temporary priorize the following databases:

- SQL
    - **Alibaba yugabyte**: A distributed SQL database that supports ACID transactions with: PostgreSQL & Cassandra CQL-like
    - **Mysql Cluster**: 
    - **Citus Data**: Postgres Inteface
	    - 
    - **TiDB**: Mysql-like
- NoSQL
    - **MongoDB**: A NoSQL database that stores data in flexible documents. It's great for applications that require a lot of flexibility.
        - Alibaba MongoShake
        - MongoDB Atlas
    - **Apache Cassandra**: A distributed database system designed to handle large amounts of data across many commodity servers.
- K-V
    - **Redis**: A fast, in-memory data structure store used as a database, cache, and message broker.
    - **DragonFly**:


### Why we need distributed?

1. To ensure high availability and fault tolerance across multiple datacenters.
2. To scale horizontally to handle large amounts of data and traffic.
3. To provide low-latency access to data for users in different regions.


### Yugabyte
#### PoC Plan 1:
The original  plan: i planned to use skupper to link 2 cluster namespace, expose master service of yugabyte of 1 cluster to each other.

I run the yugabyte db on each server successfully, however the master servers list are share between server, so we have to find another solution (Global DNS for K8s) to connect cluster together.
#### Using Pihole as External DNS

from ExternalDNS document, I found that we can use Pi-hole as external dns

https://github.com/kubernetes-sigs/external-dns/blob/master/docs/tutorials/pihole.md

