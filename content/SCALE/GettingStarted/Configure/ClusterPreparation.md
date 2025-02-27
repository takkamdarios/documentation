---
title: Preparing for Clustering
description: "Initial setup procedures to prepare a system for clustering."
weight: 70
aliases:
 - /scale/gettingstarted/clusterpreparation/
tags:
- clustering
- s3
- clustering
---



TrueNAS SCALE has the unique ability to cluster groups of systems together. These clusters can then create new volumes within the existing SCALE storage pools. Data stored in a clustered volume is shared between the clustered systems and can add additional redundancy or performance to the environment.

{{< hint type=warning >}}
Clustering is considered experimental and should not be used in a production environment or for handling critical data!
{{< /hint >}}

## Clustering Options

TrueNAS SCALE provides a few options for setting up system clustering:

* [MinIO-created distributed clustering of TrueNAS SCALE systems]({{< relref "MinIOClustering.md" >}}).

* [Clustering and sharing SCALE volumes with TrueCommand](https://www.truenas.com/docs/solutions/integrations/smbclustering/).
