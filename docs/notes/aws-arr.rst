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
    - Ex: Degradation. To ensures that services survive the failure of sub-components.


High Availability (HA) Design
------------------------------

HA is critical at every level of the LOCAL system. Any individual component that fails
must have a standby system able to take over. This is also known as building for
failover which ensures that users are always able to access a service.


AWS Physical and Networking Infrastructure
--------------------------------------------

AWS services provide high availability within their own infastructure.
This section reviews from the largest level (region) to the lowest level (AWS VPC networking)
of AWS.

- Regions & Availability Zones (AZs)
    Enables high redundant, and scalable platforms

- Virtual Private Clouds (VPCs)
- AWS VPC Networking


Appendix Notes
-----------------

- Needs vs wants
    - Consider what level of availability is required for a use case or environment
    - Think about how a disruption or data loss in that service would impact your business
    - Think about what it will take to restore service as well as what your business has committed to in its contractual obligations (requirements!!)