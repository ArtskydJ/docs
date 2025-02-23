---
layout: default
description: Oasis provides the entire functionality of ArangoDB as a service, without the need to run or manage databases yourself.
title: ArangoDB Oasis - The ArangoDB Cloud
page-toc:
  disable: true
---
![ArangoDB Oasis Logo](images/arangodb-oasis-logo-right.svg){: .center-image style="width: 66%; margin-bottom: 80px;"}

[ArangoDB Oasis](https://cloud.arangodb.com/home?utm_source=docs&utm_medium=cluster_pages&utm_campaign=docs_traffic){:target="_blank"},
the ArangoDB Cloud, provides ArangoDB databases as a Service (DBaaS).
It enables you to use the entire functionality of an ArangoDB cluster
deployment without the need to run or manage the system yourself.

The ArangoDB Cloud...

- runs your databases in data centers of the cloud provider
  of your choice: Google Cloud Platform (GCP), Amazon Web Services (AWS),
  Microsoft Azure. This optimizes performance and reduces cost.

- ensures that your databases are always available and
  healthy by monitoring them 24/7.

- ensures that your databases are kept up to date by
  installing new versions without service interruption.

- ensures that your data is safe by providing encryption &
  audit logs and making frequent data backups.

- guarantees that your data always remains your property and
  access to it is protected with industry standard safeguards.

For more information see
[cloud.arangodb.com](https://cloud.arangodb.com/home?utm_source=docs&utm_medium=cluster_pages&utm_campaign=docs_traffic){:target="_blank"}

{% assign ver = "3.10" | version: ">=" %}{% if ver %}
For quick start guide, see
[Use ArangoDB in the Cloud](../quick-start-in-the-cloud.html).
{% endif -%}
