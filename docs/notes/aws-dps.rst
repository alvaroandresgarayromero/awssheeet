.. meta::
    :description lang=en: AWS Design for Performance and Scalability
    :keywords: AWS, AWSCLI


=====================================================
Design for Performance and Scalability
=====================================================

.. contents:: Table of Contents
    :backlinks: none

Define Requirements
--------------------

Define requirements that are data-driven to measure performance:

    - Resiliency, elasticity, scalability, fault tolerance, disaster recovery, performance, security, decreased cost, centralization, cloud native applications.

Cost Optimization and Monitoring
----------------------------------

- Cloud Costs
    - These are Running Compute Resources, Storage, Provisioned Databases, Data Transfer

- Service Pricing
    - AWS Price Estimates

- Cost Management
    - EC2:
        - On demand instances: pay by the second ($$$)
        - Reserved: 1 or 3 year commitment ($$)
        - Spot: Unused instances, but AWS can terminate a spot instance with a 2-minute warning ($)
    - Storage:
        - Block Storage (Elastic Block Storage): For persistent data
        - File Storage (Elastic File System): File-based encrypted, scalable, and elastic storage
        - Object Storage S3 - General purpose scalable storage service with three tiers (hot, warm, cold -> frequent usage, infrequent access, cold storage -> S3, IA, Glacier)
    - Database:
        - Keep only a subset of data in RDS. Move videos, images, gifs, data, documents to S3

- Cost Optimization and Monitoring

    - Autoscale based on peaks (add or reduce compute resources automatically based on time, usage, and requests)
        - Vertical Scaling: Adding more resources to the system (memory and CPU)
        - Horizontal Scaling: Adding more nodes and workers (instances)
    - Use spot instances to reduce cost
    - Use private IP addresses for data transfer within the same AZ
    - Consolidate Billing from multiple accounts
    - Managing AWS cost with these tools:
        - Cost Allocation Tags: tracks costs by group, lifecyle, person, application.
        - S3 Lifecycle policies: A config file that automates and manages the life of data by moving it to less costly tiers (ex: S3->glacier->delete) in order to reduce cost
        - AWS Cost Explorer: Tool to view and analyze cost usage
        - AWS Code Guru: Tool that reviews codes and provides recommendations
        - AWS Budgets to configure custom budgets
        - AWS CloudWatch: Set threshold on services to monitor and trigger alarms
        - AWS CloudTrail: A logging service that tracks AWS service events. Can be coupled with cloudwatch to read the log metrics

Performance Optimization
-------------------------

AWS can improve performance by scaling out, scale up by allocating more memory,
optimize network routes, and failover to backup.

- Scaling
    - Burstable instances provide your application a baseline level of CPU performance with the ability to burst to a higher level when required by your workload.
    - Elasticity (automatic for some services) is the ability to acquire resources as you need them, and elasticity is ALSO releasing resources when you no longer need them.
    - Auto Scaling Instances are criteria that you configure manually for scaling up/down or scaling horizontally.

- Storage
    - Spread connection requests to maximize the accessible bandwidth
    - Access in the same AWS Region when possible to reduce latency and data transfer costs
    - Use AWS S3 Transfer Acceleration for fast and secure data transfer over long distances
    - Use Instance Store for ephemeral storage
    - Always delete unused storage volumes and obsolete

- Archive
    - S3: Hot, Immediate, general purpose ($$$)
    - S3 IA: Warm, Immediate, backups, disaster recovery ($$)
    - S3 Glacier: Cold, Up-to-several-hours, long-term, rarely accessed. ($)

- Database
    - Create read replicas to route read traffic to the copy, and offload the master database for read/write/updates/delete
    - High Performance Database use IOPS (input/output per second) SDD. Optimized for transactional workloads involving frequent read/write operations with small I/O size,
    - High Throughput Database use HDD. Optimized for large streaming workloads

- AWS Content Delivery Network (CDN)
    - AWS Cloudfront feature utilizes edge location data centers to deliver content closer to the end-user. Thus reducing latency. Cloudfront fetches the data from the origin (S3 bucket or web server), and then it gets cache into the edge cache. The data is then delivered from the cache server.

- Message Queues
    - Amazon SQS message queue is a reliable, and scalable producer-consumer buffering service that decouples microservices, distributed system, and serverless applications messaging queue.

- Networking
    - AWS Load Balancer:
        - Application Load Balances: Balances based on application performance
        - Network Load Balances: Balances based on TCP/UDP traffic patterns
        - Classic Load Balances: Balances based on multiple EC2 instances, and AZ

    - AWS CloudFront
    - Latency Routing: A configuration where requests are routed from the AWS region that provides the lowest latency.



Serverless Technology
----------------------




Infrastructure as Code
------------------------

The use of code to define your infrastructure.
This method reduces human error, and increases
automation and collaboration.


- Terraform

    - Cloud neutral IaC (supports AWS, GCP, Azure, Digital Ocean)

    - Workflow begins with a terraform (TF) config file. TF then executes based on the next states
        - Refresh - TF fetches the real-world (current state of infrastructure)
        - Plan - TF plans what needs to do done to achieve the new desired infrastructure config
        - Apply - TF applies infrastructure to the real world
        - Destroy (final day): TF removes infrastructure from the real world

    - TF main components:
        - Core
            - input: reads TF config, and state (figures out what needs to created, updated, deleted)
            - output: supports public cloud providers (AWs, GCP, etc), platform as a service (heroku, kubernetes, lambdas), Software as a Service (Github)

    - .tf is TF config file
    - .tfvars is a input variables file (similar to environment variables), which can be used to modify modules
    - TF modules abstract the infrastructure config. These are a set of TF config files in a folder.


- Collaboration and Security

    Manage terraform.tfstate with a remote backend

    - .tfstate is a terraform generated file that tracks the state of the infrastructure.
        - This file is stored locally by default or can be configure to be remotely source controlled on s3, github, terraform pro by defining backend.tf





Appendix
---------
    - Private Cloud: Organization owns, operates, and governs their cloud computing resources.
    - Community Cloud: Cloud resources provided for organizations and community groups.
    - Public Cloud: Owned by the government, academic institutions, or a business (e.g. Amazon Web Services, Microsoft Azure and Google Cloud Platform).
    - Hybrid Cloud : Combines public cloud and private cloud to allow data and resources to be shared between them.
    - Availability Zones: A logical data center in an AWS region with redundant and separate power, networking and connectivity reducing the likelihood of two zones failing simultaneously
    - AWS CloudFront: Fast content delivery network (CDN) service that securely delivers data, videos, applications, and APIs to customers globally with low latency
    - AWS Local Zones: A type of AWS infrastructure deployment that places AWS compute, storage, database, and other select services closer to large population, industry, and IT centers where no AWS Region exists today
    - AWS Regions: A geographical location with a collection of availability zones physically isolated from and independent of every other region
    - Edge Location: A physical site that CloudFront uses to cache copies of your content for faster delivery to users at any location
    - Points of Presence: AWS Edge Locations and Regional Edge Caches used for both AWS CloudFront and Lambda@Edge to deliver content to end users at high speeds
    - VPC Peering: A networking connection between two AWS VPCs that allows you to route traffic between them using private IP addresses
    - VPC Sharing: Allows you to share subnets with other AWS accounts in your organization
