.. meta::
    :description lang=en: AWS Introduction to Availability, Reliability, and Resiliency
    :keywords: AWS, AWSCLI


============================================
Availability, Reliability, and Resiliency
============================================

.. contents:: Table of Contents
    :backlinks: none

Definitions
--------------

- Availability: A measure of time that a system is operating as expected. Typically measured as a percentage.

- Reliability: A measure of how likely something is to be operating as expected at any given point in time. Said differently, how often something fails.
    - Ex: Historical Uptime. To measure how often users could utilize a service

- Resiliency (redundancy): A measure of a system's recoverability. How quickly and easily a system can be brought back online. Normally costs more both monetary and complexity
    - Ex: Degradation. To ensures that services survive the failure of sub-components. Fault-tolerant


High Availability (HA) Design
------------------------------

HA is critical at every level of the LOCAL system. Any individual component that fails
must have a standby system able to take over. This is also known as building for
failover which ensures that users are always able to access a service.


AWS Physical and Networking Infrastructure
--------------------------------------------

AWS services provide high availability within their own infrastructure.
This section reviews from the largest level (region), AZ, VCP to the lowest level (AWS VPC networking)
of AWS.

- Regions & Availability Zones (AZs)
    Enables high redundant, and scalable platforms.

    - Regions are geographically separated (interdependent), but they are connected by a global high speed private AWS network.
    - Availability Zones (AZ) is a subset of an AWS region. AZ is a physically independent building with its onw power and network connectivity.

- Virtual Private Clouds (VPCs)

    .. image:: images/vpcANDsubnets.png
       :width: 400

    - They are private networks that live within the AWS network for your needs
    - They are region specific. However, they can be configured to connect to other VPC or keep it independent.
    - VPC can have almost no cost, and you can create as many per region
    - Runs on a VPC: Anything that is instance-based will run inside a VPC
        - RDS, ElastiCache, DocumentDB, Elasticsearch, EMR, EC2, Load Balancers, Neptune, Redshift
    - Does not run on a VPC: More service-oriented AWS services tend to not run in aVPC
        - Services that are accessed directly over the internet
        - S3, DynamoDB, CloudFront, SNS, SQS, SES, Route 53, SRS API Gateway, IAM, Cloud Trail
    - Getting Started Parameters for VPC
        - Network Range: A consecutive set of IP addresses which can represented in CIDR notation. ex: 10.2.0.0/24 means 10.2.0.X where [3-bytes, 24-bits = 10.2.0], and .X is 1-byte word of 255 possible addresses
        - Note: Two VPCs can't overlap network ranges, but if they do overlap they cannot pair them to each other

- AWS VPC Networking
    Similar to traditional network configuration there are a variety of way to set up incoming and outgoing traffic from the VCP.

    - Subnet - Tied to an AZ
    - Route Table - Attach to one of more subnet for instance data connectivity
    - Managing security is important with your own VCP. AWS has several options to connect to the outside world
        - Internet Gateway: I/O from VPC to the internet
        - NAT Gateway: Allows outbound only connections from your VPC to the internet
        - VPN/direct fiber: Allows connection to your own datacenter.
        - Security Group is like a firewall that can be attached to instances

Building for Resiliency
-------------------------

- AWS Server-based Services:
    A service is a server/instance based service if the service is a pre-existing product
    that AWS has created a service with  (MongoDB, Redis, MySQL, Postgres, Docker, Kubernetes)
    - Do not come with any fault tolerance by default. Therefore, you need to tell the service to provision a second instance for the primary instance to failover to.

    - Multi-AZ redundancy:
        - Subnet Groups define the different availability zones that your service will run in
        - Multiple instances (a hot-stand-by ready to take over) allow for fast failover if a single AZ were to go down.

    - Multi-Region redundancy:
        -More tricky, harder, and may not be possible to run a service with failover between regions.

- AWS DynamoDB: AWS NonRational Database
    - Comes with fault tolerance because it is a multi-AZ by default so the cost is already baked-in by default.
    - DynamoStreams captures changes done to the table. Similar to flask-migrate which tracks changes done to the database via sqlalchemy in python.
    - DynamoDB Global Tables orchestrates multi-region tables couple DynamoStreams. Any changes propagates to any region.

- AWS S3: Simple Storage Service
    - Multi-Regional by default

    - Create buckets to store unlimited numbers of objects in a bucket
        - Standard: For objects that are accessed frequently
        - Infrequently Accessed (IA): For objects that are used occasionally
        - Intelligent Tiering: For objects with undefined access times
        - Glacier: For objects that are used for backup or unlikely to use often
        - Glacier Deep Archive: Even colder storage, for items that you need keep but will rarely access. It can take 12 hours to get the data.

    - S3 Features:
        - Lifecycle policy is used to move or delete objects based on the time that they have been in a bucket. Ex: an object that moves to glacier after not been used in a year
        - SE Events such as CUD (Create, Update, Delete) events can be monitor with lambda functions for further data analysis
        - S3 Versioned Buckets can be used to version changes, and even revert deleted objects


- AWS Compute Services:
    Similar to server-based services in that they are not Multi-AZ by default, but they can be configured.

    - Compute Services are:
        - EC2 & Container Services
            - coupled with AutosScaling Group (ASGs) can be used to maintain multiple instances in multi-AZ
            - coupled with Elastic Load Balancers, your instances can support income HTTP requests. The load balancer can pass the request to multi-AZ
        - LAMBDA: serverless, and standalone software

Business Objectives
--------------------

Requirements/agreements (also known as Service Level Agreements - SLAs) between the developer and business must be clearly defined
in order to have common, measurable, and achievable goals

- Uptime:
    - Percent Measurement of how much time an application or service is available and running normally
    - Percentage Chart: https://en.wikipedia.org/wiki/High_availability
    - 99% Uptime (hours/month) = ( 60 mins * 24 hr * 30 days ) * .99 = 42768 mins => 712 hours per month

- Downtime
    - Opposite of Uptime
    - Downtime = 99% Uptime (hours/month) = ( 60 mins * 24 hr * 30 days ) * .01 = 432 mins => 7.2 hrs per month
    - DownTime Exceptions to add to contract:
        - Scheduled Maintenance - Free pass on downtime
        - Degrade Service - Nice to have feature that are not really required. These may be excluded from the Uptime requirement thus easing the complexity.
        - Force Majeure - Natural disasters or pandemics or acts of God. Free pass on downtime.

- RTO (Recovery Time Objective)
    - The maximum time your platform or service can be unavailable

- RPO (Recovery Point Objective)
    - The maximum amount of time that your system can lose data for. It is the window of time that you may lose data

- Disaster Recovery (Worst case scenario)
    - Disaster recovery (DR) usually involves the wholesale moving of your platform from one place to another.
    - Types of DR plans from least readiness to full readiness
        - Cold standby: Have all data in a DR region, but no services running...so they all have to be spun up.
        - Pilot Light, Warm Standby
        - Hot standby: Have a complete DR region (clone) with all services and data ready to take over (expensive)
    - Types of AWS Features for DR issues (multi-region services)
        - DynomoDb and S3 can be made multi-region
        - RDS can make cross-region replicas
        - IAM is a global service and thus multi-region by default
        - Cloudfront coupled with CloudFront-Origin-Groups can create data fault tolerance by linking to a primary S3 bucket in one region, and a backup S3 bucket in another region.

Monitor, React, and Recover
----------------------------

- Monitoring
- Alerting
- Recovering
- Automating


Appendix Notes
-----------------

Understanding how resilient to make a system is critical when approaching anew product or service.

- Needs vs wants
    - Consider what level of availability is required for a use case or environment
    - Think about how a disruption or data loss in that service would impact your business
    - Think about what it will take to restore service as well as what your business has committed to in its contractual obligations (requirements!!)
    - When approaching a new product or service, you should consider how important the system will be. Will the existence of the company depend on it staying up, or is it just helping a team vote on lunch choices?


Glossary
----------

    - Active/Active: A system that is running actively in multiple instances, typically in a distributed manner where complete functionality is available in more than one area.
    - Snapshot: A complete copy of a dataset at a specific point in time.
    - Server-based Services: Services that are existing applications that AWS provides as "managed services" and run on individual server instances.
    - DynamoDB: AWS developed non-relational database
    - DynamoDB Global Tables: Multi-Region DynamoDB Tables.
    - S3: AWS developed object store that can store an unlimited amount of data.
    - Compute Services: AWS services that provide generic compute capacity.`
    - Durability: A measurement that the data won't be lost
    - Force Majeure: Term describing an event or circumstance that is completely unavoidable.
    - IP whitelisting: Allowing specific IP addresses only to access some resource.