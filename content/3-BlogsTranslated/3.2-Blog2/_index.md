---
title: "Blog 2"
date: 2025-01-01
weight: 1
chapter: false
pre: " <b> 3.2. </b> "
---

# Querying Amazon S3 Tables from Open-Source Trino Using the Apache Iceberg REST Endpoint

Authors: Aneesh Chandra PN, Aritra Gupta, Ananta Khanal, and Madhu Nagaraj

Publication date: June 13, 2025

Source: Advanced (300), Amazon EC2, Amazon S3 Tables, Amazon Simple Storage Service (S3), Amazon VPC, AWS CLI, AWS CloudFormation, AWS Identity and Access Management (IAM), Technical How-to

---

Organizations are increasingly focused on tackling the growing challenge of managing and analyzing massive volumes of data, while ensuring their data teams have timely access to that data so they can quickly generate insights and make decisions. Analysts and data scientists need self-service analytics capabilities to build and maintain data products, which often involve complex transformations and frequent updates. However, this creates significant operational overhead—from managing small files and delete markers, to handling ever-expanding metadata and rising storage costs for historical versions. If left unaddressed, these challenges can hurt query performance and increase infrastructure costs.

Modern data architectures have evolved to address these challenges using powerful open-source technologies such as **Trino** and  **Apache Iceberg** , combined with managed services like  **Amazon S3 Tables** . Trino excels at distributed querying across a wide variety of data sources, while Apache Iceberg provides a robust table format with ACID transactions, schema evolution, and time travel. S3 Tables complement this stack by providing automatic optimization and maintenance for Iceberg tables, delivering consistent performance without manual intervention.

In this post, we show how to integrate Trino with S3 Tables to create a powerful analytics platform that combines the best of both technologies. We walk through the setup process using the S3 Tables table-management APIs that are compatible with the Apache Iceberg REST catalog standard, illustrating how to configure the integration with Trino and enable key benefits such as automatic data compaction and snapshot management. By the end of the post, you’ll see how to use this powerful combination to build and maintain high-performance data products at scale.

This post focuses on configuration for the S3 Tables Iceberg REST endpoint. You can also use S3 Tables with the AWS Glue Iceberg REST endpoint. For unified governance across all tabular data, fine-grained access control, and centralized data management, we recommend using  **Amazon SageMaker Lakehouse** , which supports S3 Tables as a linked catalog.

---

## Solution Overview

The S3 Tables Iceberg REST endpoint provides a standards-based API that allows Trino to communicate directly with S3 Tables without any proprietary connector or additional middleware. This means your Trino deployments can immediately take advantage of the built-in S3 Tables optimizations for Iceberg workloads.

This guide will show you how to:

* Deploy a single-node Trino environment using an **AWS CloudFormation** template
* Configure the Iceberg REST connector to communicate with S3 Tables
* Create schemas and tables directly in S3 Tables from Trino
* Perform full-fidelity Iceberg read/write operations
* Use advanced features such as time travel and schema evolution

**Deployment architecture:** Although Trino is typically deployed as a multi-node distributed architecture for production workloads to leverage parallel processing, this post uses a single **Amazon Elastic Compute Cloud (Amazon EC2)** instance so that you can quickly explore the S3 Tables–Trino integration. The Iceberg catalog configuration will be the same when you deploy on your own Trino cluster for production use cases.

---

## Prerequisites

You need the following to complete this solution:

* An AWS account with permissions to create resources such as EC2 instances, AWS Identity and Access Management (IAM) roles, and S3 table buckets
* An Amazon Virtual Private Cloud (Amazon VPC) with a public subnet (or a private subnet with VPN access)
* An Amazon EC2 key pair for SSH access
* The AWS Command Line Interface (AWS CLI) installed and configured (optional, used for manually creating the S3 table bucket)

---

## Step-by-Step Guide

The following sections walk you through deploying this solution.

---

## Part A: Deploy Trino with S3 Tables Integration

This section provides step-by-step instructions for launching a Trino environment using the supplied CloudFormation template. It also describes the resources required to build this solution. To simplify integration between Trino and S3 Tables, we’ve created a CloudFormation template that automates the entire deployment process.

### 1. CloudFormation Template Overview

The CloudFormation template provisions all the components required for a complete Trino environment that can read and write data to S3 Tables through the Iceberg REST endpoint:

* **S3 table bucket:** Creates a dedicated bucket to store your data
* **EC2 instance:** Deploys a single-node Trino server using version 475
* **Security group:** Configures network access for SSH and the Trino web UI
* **IAM instance profile:** Grants the EC2 instance the appropriate permissions

### 2. Deployment Process

1. Download the CloudFormation template.
2. In the CloudFormation console, choose **Create stack > With new resources (standard)** as shown in the screenshot.
3. Upload the template file and choose  **Next** .
4. Provide values for the template parameters:
   * **Stack name:** A name for your CloudFormation stack
   * **KeyName:** An existing EC2 key pair for SSH access
   * **VpcId:** The VPC in which to deploy the EC2 instance
   * **SubnetId:** A public subnet in your VPC
   * **S3TablesBucketName:** A name for your S3 table bucket
   * **AwsRegion:** The AWS Region for S3 Tables and AWS Glue
   * **TrinoInstanceType:** EC2 instance size (default: `t3.xlarge`)
5. Choose **Next** on the stack options page.
6. Review the details, confirm that CloudFormation can create IAM resources, and then choose  **Create stack** .
7. Wait for stack creation to complete (about 10–15 minutes).

When the stack is complete, you can find useful information in the **Outputs** tab:

* **PublicDNS:** The public DNS name of your Trino instance
* **SSHCommand:** The SSH command you can use to connect to the instance
* **TrinoURL:** The URL for the Trino web UI
* **TableBucketName:** The name of your S3 table bucket

### 3. What the Deployment Does

The CloudFormation template performs several key tasks:

* **Infrastructure provisioning:** Sets up the EC2 instance, security group, and S3 table bucket
* **Software installation:** Installs Amazon Corretto Java 23 and Trino 475
* **Configuration:** Creates the required Trino configuration files
* **Integration setup:** Configures the Iceberg REST connector for S3 Tables

---

## Part B: Connecting Trino to Amazon S3 Tables Using the Iceberg REST Endpoint

The CloudFormation template automatically configures the S3 Tables catalog in Trino. In this section, we’ll look at the configuration that enables this integration.

### 1. Catalog Configuration Details

A catalog in Trino is a configuration that allows access to a specific data source. A single Trino cluster can be configured with multiple catalogs, providing simultaneous access to different data sources.

In this setup, the CloudFormation template creates a catalog properties file at:

`/home/ec2-user/trino-server-475/etc/catalog/s3tables_irc.properties`

with the following configuration:

```properties
connector.name=iceberg
iceberg.catalog.type=rest
iceberg.rest-catalog.uri=https://s3tables.${AwsRegion}.amazonaws.com/iceberg
iceberg.rest-catalog.warehouse=arn:aws:s3tables:${AwsRegion}:${AWS::AccountId}:bucket/${S3TablesBucketName}
iceberg.rest-catalog.sigv4-enabled=true
iceberg.rest-catalog.signing-name=s3tables
iceberg.rest-catalog.view-endpoints-enabled=false
fs.hadoop.enabled=false
fs.native-s3.enabled=true
s3.iam-role=<ARN of the IAM ROLE with permissions to S3 Tables>
s3.region=${AwsRegion}
```

### 2. S3 Tables Iceberg REST Endpoint Configuration Properties

The table below lists the key properties in the Trino catalog configuration:

| Property                                        | Description                                                                                                                                                |
| ----------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `iceberg.rest-catalog.uri`                    | REST server API endpoint URI (**required** ) —`https://s3tables.${AwsRegion}.amazonaws.com/iceberg`                                               |
| `iceberg.rest-catalog.warehouse`              | Warehouse identifier/location for the catalog (**required** ). For S3 Tables, this is the ARN of the S3 table bucket, as shown in the example above. |
| `iceberg.rest-catalog.sigv4-enabled`          | Must be set to `true`( **required** )                                                                                                              |
| `iceberg.rest-catalog.signing-name`           | Must be set to `s3tables`( **required** )                                                                                                          |
| `iceberg.rest-catalog.view-endpoints-enabled` | Must be set to `false`( **required** )                                                                                                             |
| `fs.hadoop.enabled`                           | Must be set to `false`                                                                                                                                   |
| `fs.native-s3.enabled`                        | Must be set to `true`                                                                                                                                    |
| `s3.iam-role`                                 | Amazon Resource Name (ARN) of the IAM role with permissions to S3 Tables. In this post, we use the same role attached to the EC2 instance.                 |
| `s3.region`                                   | AWS Region, for example `us-east-1`                                                                                                                      |

This configuration establishes the connection between Trino and the S3 Tables REST endpoint. You can register multiple catalogs, each mapped to a different S3 table bucket, identified by the `iceberg.rest-catalog.warehouse` property.

---

## 3. Working with S3 Tables in Trino

After you’ve set up and configured Trino to work with S3 Tables, you can start exploring the integration.

### 3.1. Connecting to Trino

1. Add your desired IP or subnet to the inbound rules of the Trino security group to allow SSH access (port 22).
2. Connect to the EC2 instance using the SSH command provided in the CloudFormation stack outputs:

   ```bash
   ssh -i your-key.pem ec2-user@your-instance-public-dns
   ```
3. Once connected, use the Trino CLI, which the CloudFormation template installed automatically:

   ```bash
   cd /home/ec2-user
   ./trino-cli --catalog s3tables_irc
   ```

   This command connects you to the Trino server using the S3 Tables catalog configuration you set up.

### 3.2. Example: Creating and Querying a Table

In this section, you’ll run a few example queries to demonstrate how the system works.

#### 3.2.1. Create a Namespace

First, create a namespace (schema) in S3 Tables. A namespace in S3 Tables is a logical container or organizational unit that groups related tables and objects.

```sql
CREATE SCHEMA blog_namespace;
USE blog_namespace;
```

#### 3.2.2. Create a Table

Create a table with a variety of data types. You don’t need to specify the table type as Iceberg because you’re connecting to an Iceberg catalog. You can use all standard Iceberg capabilities such as partitioning and sorting.

In addition, several important Iceberg table properties that support table maintenance operations are configured with default values. You can also modify these settings using the  **S3 Tables maintenance APIs** .

```sql
CREATE TABLE IF NOT EXISTS customers (
  customer_sk INT,
  customer_id VARCHAR,
  salutation VARCHAR,
  first_name VARCHAR,
  last_name VARCHAR,
  preferred_cust_flag VARCHAR,
  birth_day INT,
  birth_month INT,
  birth_year INT,
  birth_country VARCHAR,
  login VARCHAR
) WITH (
  format = 'PARQUET',
  sorted_by = ARRAY['customer_id']
);

SHOW TABLES;

DESCRIBE customers;
```

#### 3.3.3. Insert Data

You can insert some sample data into the table. You can also use an existing table in any catalog already configured in Trino and populate an S3 Table using an `INSERT INTO .. SELECT` statement.

```sql
INSERT INTO customers
VALUES
    (1, 'AAAAA', 'Mrs', 'Amanda',     'Olson',  'Y', 8,  4, 1984, 'US', 'aolson' ),
    (2, 'AAAAB', 'Mr',  'Leonard',    'Eads',   'N', 22, 6, 2001, 'US', 'leads' ),
    (3, 'BAAAA', 'Mr',  'David',      'White',  'Y', 16, 2, 1999, 'US', 'dwhite' ),
    (4, 'BBAAA', 'Mr',  'Melvin',     'Lee',    'N', 30, 3, 1973, 'US', 'mlee' ),
    (5, 'AACAA', 'Mr',  'Donald',     'Holt',   'N', 2,  6, 1982, 'CA', 'dholt'),
    (6, 'ABAAA', 'Mrs', 'Jacqueline', 'Harvey', 'N', 5, 12, 1988, 'US', 'jharvey'),
    (7, 'BBAAA', 'Ms',  'Debbie',     'Ward',   'N', 6,  1, 2006, 'MX', 'dward'),
    (8, 'ACAAA', 'Mr',  'Tim',        'Strong', 'N', 15, 7, 1976, 'US', 'tstrong'
);
```

#### 3.3.4. Query the Data

You can query the data you just inserted with:

```sql
SELECT * FROM customers LIMIT 10;
```

---

### Create Table As Select (CTAS)

Trino also supports creating a table from the result of a query. In this example, you create a copy of the previous table:

```sql
CREATE TABLE trino_customers
WITH (
  format = 'PARQUET'
)
AS SELECT * FROM customers;

SELECT * FROM trino_customers LIMIT 10;
```

---

### Altering a Table

One of the benefits of using Iceberg is  **schema evolution** . You can add a new column and then populate it. Each transaction from Trino (such as an `ALTER` DDL statement) creates a new Iceberg table snapshot.

```sql
ALTER TABLE trino_customers ADD COLUMN updated_at TIMESTAMP;

DESCRIBE trino_customers;

UPDATE trino_customers SET updated_at = current_timestamp;

SELECT * FROM trino_customers LIMIT 10;
```

---

## 3.4. Advanced Iceberg Features

Trino provides access to Iceberg’s metadata, allowing you to inspect information about the objects stored in S3 Tables. This makes it easier to understand how data is organized and what’s associated with each table transaction.

### 3.4.1. Querying Table Metadata

```sql
-- View snapshot information
SELECT * FROM "trino_customers$snapshots";

-- View data file information
SELECT * FROM "trino_customers$files";
```

### 3.4.2. Time Travel

**Time travel** is a powerful feature of the Apache Iceberg table format that lets you query data as of a specific point in the past. This is especially useful for analytics, auditing, and reproducing historical results.

Trino supports time travel using the `FOR VERSION AS OF` syntax, where you can provide a snapshot ID or a timestamp, as shown below:

```sql
-- Time travel using a snapshot ID (replace <snapshot ID> with an actual ID from the snapshots query above)
SELECT * FROM trino_customers FOR VERSION AS OF <snapshot ID>;

-- Time travel using a timestamp
SELECT * FROM trino_customers FOR TIMESTAMP AS OF TIMESTAMP '2025-03-13 08:00:00.000 UTC';
```

### 3.4.3. Viewing History and Rolling Back

Iceberg’s **history and rollback** features provide powerful data-versioning capabilities. You can view the full history of operations on a table and easily revert to a previous state using a timestamp or snapshot ID, which helps ensure data recovery and auditing in your data lake.

```sql
-- Delete a record
DELETE FROM trino_customers WHERE customer_sk = 8;

-- View the table history
SELECT * FROM "trino_customers$history";

-- Roll back to a previous snapshot (replace <snapshot ID> with the ID from before the delete)
ALTER TABLE trino_customers EXECUTE rollback_to_snapshot(<snapshot ID>);
```

Now for the fun part—we’ve run several transactions with the example queries above and seen how Iceberg creates a snapshot for each transaction (or table operation). In real-world scenarios, you may need to update data frequently, or continuously load small batches of data into a table every few seconds or minutes. These operations often create many small files, which can degrade performance, cause duplicated data across multiple snapshots, and leave behind expired data (older versions of records that have been updated or deleted).

S3 Tables provide maintenance operations such as  **data compaction** ,  **snapshot management** , and **orphan-file cleanup** to keep tables optimized and reduce storage costs by removing unnecessary files. These options are enabled by default for all tables with preconfigured properties, and you can also customize them at the individual table level based on your requirements. You can learn more about S3 Tables maintenance in the documentation.

---

## Cleanup

To clean up resources, complete the following steps:

1. **Delete data and tables** (you can do this directly from the Trino CLI, avoiding the need for the AWS CLI).
2. In the AWS console, go to **CloudFormation** and delete the stack you created.

---

## Conclusion

In this post, we demonstrated seamless integration between Trino and Amazon S3 Tables using the Iceberg REST endpoint. This powerful combination lets you use Trino’s distributed query engine to perform interactive analytics on data stored in S3 Tables, while benefiting from Iceberg features such as transactional support, schema evolution, and time travel—along with the built-in Iceberg optimizations provided by S3 Tables.

This solution delivers a flexible, high-performance platform for modern data analytics on AWS. By automating deployment with CloudFormation and leveraging the standard Iceberg REST interface, you can quickly set up this integration and start extracting value from your data. Whether you’re building a new data platform or extending an existing system, the combination of Trino and S3 Tables offers a solid foundation for analytical workloads at scale.

To learn more, see:

* The guides for using S3, Trino, and the Apache Iceberg usage and configuration documentation
* Additional S3 Tables posts on the AWS Storage Blog

**Keywords:** Amazon Elastic Compute Cloud (Amazon EC2), Amazon Simple Storage Service (Amazon S3), Amazon VPC, AWS Cloud Storage, AWS CloudFormation, AWS Command Line Interface (AWS CLI), AWS Identity and Access Management (IAM)

---

### About the Authors

**Aneesh Chandra PN**

Aneesh Chandra PN is a Principal Analytics Solutions Architect at AWS working with Strategic customers. He is passionate about leveraging technology advancements to solve customers’ data challenges. He uses his deep expertise in analytics, distributed systems, and open-source frameworks to act as a trusted technical advisor to AWS customers.

**Aritra Gupta**

Aritra Gupta is a Senior Technical Product Manager on the Amazon S3 team at AWS. He helps customers build and scale multi-Region architectures on Amazon S3. Based in Seattle, he enjoys playing chess and badminton in his free time.

**Ananta Khanal**

Ananta Khanal is a Senior Solutions Architect at AWS focused on data and cloud-computing solutions. He has worked in IT for more than 15 years and has held various roles across different companies. He is passionate about cloud technologies, infrastructure management, IT strategy, and data management.

**Madhu Nagaraj**

Madhu is a Senior Technical Account Manager (TAM) at Amazon Web Services (AWS) with more than 20 years of experience in IT, including software engineering, cloud operations, and automation. Since joining AWS in 2021, he has been passionate about helping enterprise customers navigate and adopt emerging technologies. He currently focuses on helping customers innovate with Generative AI and AI Agents, combining strong technical capabilities with cost optimization and operational efficiency. Outside of work, Madhu enjoys spending time with his family and hiking.
