---
sort: 1
---

# Multi-region web app with private connectivity to a database

This is a very common [pattern](https://learn.microsoft.com/en-us/azure/architecture/example-scenario/sql-failover/app-service-private-sql-multi-region) for deploying typical Line Of Business Applications.

A key feature of this specific Architecture is that there is no connectivity to the wider estate.

![Alt text](https://learn.microsoft.com/en-us/azure/architecture/example-scenario/sql-failover/media/app-service-private-sql-multi-region-solution-architecture.svg#lightbox)

## Zone Redundancy & DevOps

A [variation/extension](https://learn.microsoft.com/en-us/azure/architecture/web-apps/app-service/architectures/baseline-zone-redundant#alternatives) of this Architecture showing Zone Redundancy & DevOps deployment is shown below.

![Alt text](https://learn.microsoft.com/en-us/azure/architecture/web-apps/app-service/_images/baseline-app-service-network-architecture.svg#lightbox)