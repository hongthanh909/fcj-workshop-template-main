---
title: "Blog 1"
date: 2025-06-13
weight: 1
chapter: false
pre: " <b> 3.1. </b> "
---
# How to SAN Boot Enterprise Amazon EC2 Environments from Amazon FSx for NetApp ONTAP

**Author:** Randy Seamans
**Published:** June 13, 2025
**Source:** Advanced (300), Amazon EC2, Amazon Elastic Block Store (Amazon EBS), Amazon FSx for NetApp ONTAP, AWS Partner Network, Migration, Storage, Thought Leadership

---

## Introduction

Traditionally, many enterprises and organizations using on-premises infrastructure have deployed boot-from-SAN (Storage Area Network) instead of using locally attached storage. Booting from SAN provides centralized management and backup of boot volumes, supports high availability through multipathing, and offers greater flexibility by allowing systems to boot from pre-configured OS images stored on shared storage arrays to reduce costs.

Amazon FSx for NetApp ONTAP brings these benefits to the cloud. As a fully managed service by Amazon Web Services (AWS), FSx for ONTAP provides an enterprise-grade virtual storage array supporting features such as high-throughput I/O, deduplication, compression, compaction, replication, and block-level access through iSCSI and NVMe/TCP.

Most importantly for SAN boot is the thin cloning capability. FSx for ONTAP allows a thinly provisioned LUN to act as a base "golden image" for the operating system (OS). Read-write snapshot clones of this LUN can be rapidly provisioned and distributed to hundreds of servers as separate boot volumes. Each clone only stores the minimal differences to identify each server, significantly reducing total storage capacity requirements.

Furthermore, FSx for ONTAP is aware of shared data regions, allowing frequently accessed blocks to be cached in memory once and served to all clones, effectively extending cache efficiency and improving overall performance. Since FSx for ONTAP also provides high availability (HA) and disaster recovery (DR) through advanced replication mechanisms, boot volumes can be integrated into HA/DR workflows. This ensures OS state remains consistent across environments without manual intervention.

---

## Background on Boot Devices in AWS

AWS instances typically boot from Amazon Elastic Block Store (Amazon EBS) volumes, which are tightly integrated with Amazon Elastic Compute Cloud (Amazon EC2). This integration enables fast and stable boot times through features like EBS Fast Snapshot Restore and EBS Provisioned IOPS for volume initialization.

Amazon EBS also provides enhanced security with customer-managed key (CMK) encryption, high reliability through independently operating boot volumes, and time-based AMI copies for efficient and consistent distribution across Regions. Designed for both general-purpose and high-performance workloads, Amazon EBS is the default boot device for Amazon EC2.

In this article, I will demonstrate how you can boot from iSCSI LUNs stored on FSx for ONTAP file systems in either Single-Availability Zone (AZ) or Multi-AZ configurations. These LUNs can be thin provisioned, space-efficient, and replicated between AZs or different AWS Regions.

When properly configured, SAN booting from FSx for ONTAP can help reduce storage costs at scale while simplifying HA/DR operations.

---

## Two Primary Use Cases for SAN Boot with FSx for ONTAP

### 1. Reducing Boot Volume Costs

In on-premises environments, SAN boot is commonly used to reduce costs when deploying hundreds of servers with nearly identical boot volumes. This principle also applies in the cloud when using iSCSI boot with FSx for ONTAP.

By leveraging thin provisioning and snapshot-based cloning, the storage requirements for 100 to 200 boot volumes are only marginally higher than a single boot volume. Each server only consumes capacity for its unique differences from the golden image, dramatically reducing overall storage usage.

Furthermore, by following the best practices mentioned later in this article, you can avoid provisioning dedicated IOPS for boot volumes thanks to FSx for ONTAP's performance pooling capability. The result is significant cost savings while maintaining consistent performance.

### 2. Simplifying HA/DR and OS Lifecycle Management

OS updates and configuration changes are frequent requirements in enterprise workloads. Using SAN boot optimizes HA/DR workflows by replicating boot volumes across multiple Availability Zones (AZs) and remote AWS Regions.

FSx for ONTAP supports both multi-AZ replication and long-distance replication, so any changes to the OS or boot volume are automatically synchronized and always in a highly available state. This significantly reduces the manual steps required for recovery during incidents while limiting the risk of human error, making it easier to meet stringent recovery time objectives (RTO).

Additionally, updates can be deployed first on a clone of the golden image, thoroughly tested, and only promoted to production once validated, streamlining the OS update process and minimizing disruption.

---

## How SAN Boot from FSx for ONTAP Volumes Works

To perform SAN boot from FSx for ONTAP, we use the concept of a network-based chain-loader boot device, sometimes called "jumpboot."

Initially, the EC2 instance quickly boots a compact, locked-down OS from a 1 GB EBS volume containing the Preboot eXecution Environment (iPXE). iPXE then chain-boots to a volume storing the actual Linux or Windows OS located on FSx for ONTAP.

Users can compile their own iPXE Amazon Machine Image (AMI) or use the AWS certified iPXE AMI available in every AWS Region as a community AMI. This chain-loading mechanism still allows integration with the Amazon EC2 console for operations like launch, start, stop, or using the serial console.

How does iPXE know which FSx for ONTAP and iSCSI volume to boot from? When launching an EC2 instance with the iPXE AMI, we pass that information in the user data script, and iPXE then chain-boots the new OS stored on the specified block volume.

---

## Practical Considerations and Best Practices

Booting from SAN using FSx for ONTAP in AWS brings several considerations for planning and operations, similar to traditional on-premises SAN environments, with some cloud-specific best practices.

### OS Licensing

Boot volumes are typically cloned, so each instance must fully comply with corresponding licensing requirements, especially for commercial operating systems like Microsoft Windows.

### Storage Placement

Unless there are specific requirements otherwise, it's best to place both boot volumes and data volumes for an EC2 instance on the same FSx for ONTAP system. This ensures optimal data locality and maintains consistent performance.

### Avoiding Boot Storms

Another important best practice is avoiding concentrating too many boot volumes on a single FSx for ONTAP system. In large-scale recovery scenarios, often called "boot storms," this can lead to boot time delays.

Fortunately, unlike traditional on-premises systems, in AWS there's virtually no significant cost difference when distributing the same storage capacity across multiple FSx for ONTAP systems. This allows you to scale horizontally without incurring major additional costs, ensuring boot storms are avoided.

For example, an FSx for ONTAP system with 50 TB SSD capacity, by default and without provisioning dedicated IOPS, can achieve up to 150,000 IOPS. If this system supports SAN boot for 200 servers, during the boot phase each server would average 750 IOPS per second—more than six times faster than a typical HDD. Because applications and boot are co-located, there's no application IO contending with the boot process during startup.

### Multipathing Configuration

To prevent disruption, ensure multipathing is correctly configured and validated for all boot volumes connecting via iSCSI. Reliable path failover mechanisms are critical for maintaining both performance and fault tolerance.

### Testing HA and Failover

Finally, testing HA and failover configurations before production is extremely important. You can simulate a failover event by temporarily increasing the throughput capacity of the FSx for ONTAP system. This triggers a non-disruptive controller failover, allowing you to verify multipath handling and OS stability. Once confirmed successful, you can reduce throughput back to the required level.

---

## Getting Started

There are quite a few steps involved in setting up SAN boot OS in AWS with FSx for ONTAP. The best way to start is to explore the iPXE website page dedicated to AWS, and/or contact AWS directly if your organization needs to deploy SAN boot.

For large-scale migration projects to SAN boot from on-premises environments or from an existing AWS environment, Cirrus Data—an AWS Partner—provides solutions that automate the entire process, including iSCSI, multipathing configuration, provisioning, and LUN mapping, all of which are complex tasks at scale.

---

## Is SAN Boot from FSx for ONTAP Right for You?

Booting from SAN using FSx for ONTAP block storage isn't the choice for most AWS environments, but for organizations already accustomed to using SAN boot to simplify DR processes or orchestrate large-scale infrastructure with uniform OS images, this capability is now available on AWS.

If you're managing a large fleet of EC2 instances requiring HA capability, failover between AZs, or cross-Region replication, FSx for ONTAP will help you significantly reduce boot volume costs while optimizing failover and DR workflows.

In summary, you can absolutely apply proven SAN boot strategies in the cloud environment with AWS's scale and durability.

---

## Tags

Amazon EC2, Amazon FSx for NetApp ONTAP, AWS Partner Network, AWS Storage, Data Migration

---

## About the Author

**Randy Seamans** is a veteran storage industry expert and Principal Storage Specialist & Advocate for AWS, specializing in High Performance Storage (HPC/AI), Enterprise Storage, and Disaster Recovery.

For more insights and fun about Storage, follow him at: https://www.linkedin.com/in/storageperformance
