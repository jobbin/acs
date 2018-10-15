# Limits for swarm clusters {#concept_ivl_vfb_5db .concept}

The limits for Alibaba Cloud Container Service swarm clusters are as follows.

## Cluster {#section_ngj_zfb_5db .section}

-   By default, you can create at most five clusters in all regions and add at most 20 worker nodes in each cluster. To create more clusters or add more nodes to a cluster, open a ticket.
-   The Elastic Compute Service \(ECS\) instances and Server Load Balancer instances created with clusters only support the Pay-As-You-Go billing method.


## Expand a cluster {#section_khw_xfb_5db .section}

The nodes added by expanding a cluster are Pay-As-You-Go nodes.

## Add an existing ECS instance {#section_ub1_ygb_5db .section}

-   The ECS instance to be added must be in the same region and use the same network type \(Virtual Private Cloud \(VPC\)\) as the cluster.
-   When adding an existing ECS instance, make sure that your ECS instance has an Elastic IP \(EIP\) for the network type VPC. Otherwise, the ECS instance fails to be added.
-   The ECS instance to be added must be under the same account as the cluster.

## Bind a Server Load Balancer instance {#section_ltg_zgb_5db .section}

-   You can only bind a Server Load Balancer instance to a cluster of the same region.
-   You can only bind a Server Load Balancer instance to a cluster created by the same account.
-   A VPC cluster can bind an Internet Server Load Balancer instance or an intranet Server Load Balancer instance.
-   One cluster can only bind one Server Load Balancer instance.
-   Two clusters cannot share one Server Load Balancer instance.

