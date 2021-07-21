.. meta::
    :description lang=en: AWS Design for Security
    :keywords: AWS, AWSCLI


=====================================================
Design for Security
=====================================================

.. contents:: Table of Contents
    :backlinks: none

Definition
-----------

AWS is responsible for security of the cloud:

    - COMPUTE, STORAGE, DATABASE, NETWORKING, HARDWARE (REGIONS, AZ, EDGE LOCATIONS)

Customer (developer, cloud architect,etc) is responsible for security in the cloud:

    - Customer data, platform, applications, identity and access management, OS, networking traffic

This section will focus on valuable steps to increase security, and reduce risk of misconfigurations and breach


Identify and Access Management (IAM)
--------------------------------------

This section focuses on how to use IAM roles to provide secure access to both users and application that need to use AWS services.

    - Key Questions:
        - How users are managed? Is there a central trusted identify that manages user provisioning, deprovisioning, credential enforcement, and role segmentation?
        - Is Identify federation in place so the organization as a whole governs user role and access?
        - Are users part of the correct groups allowing actions needed for their roles only?
        - Are permissions for these roles fine tuned to be the least privilege in order to reduce the risk if an user is compromised.
        - Are temporary credentials been used?
        - What's the worst case scenario if credentials were stolen?

    - AWS IAM
        - Authentication are defined through AWS IAM
        - Authorization (Permissions): IAM user and roles are similar. The main difference is that users get access keys, and roles are temporary access with no key
            - IAM users: Defined through the IAM user policy. Each user is provided with keys that can be saved into a config file or code. Recommended only for Authentication
            - IAM roles: Defined through the IAM role policy. Credentials are temporary and there is no need to store API keys permanently. Recommended for Authorizations
        - The image below shows an example of an user getting access into AWS and cloud services with IAM user policy

        .. image:: images/IAM_diagram.png
           :width: 400

        - The image below shows an example of an user getting access into AWS with IAM user, and IAM roles to access cloud services

        .. image:: images/devOps_user_access.png
           :width: 400

        - The image below shows an example of an AWS service getting access to other AWS services via IAM roles. No keys are required to be maintained. It is also possible to do the same with IAM users, but not recommended.

        .. image:: images/developer_creating_app_access.png
           :width: 400

    - AWS Control Plane and Access Model
        - Control plane enables the interface to provision and configure services




    - The Importance of Identity and Access Management Best Practices


    - Using IAM roles and Identity Federation to Provide Secure Access to Users or Applications
        - Provisioning, configuring, and accessing cloud resources and data is achieved via the API through the AWS console or AWS CLI.

    - Least Privilege Permissions and Policies




Securing Access to Cloud Infrastructure
--------------------------------------

This section focuses on technique to secure the cloud infrastructure such as
access to instances, servers, and cloud network.



Data Protection in the Cloud
-----------------------------

This section focuses on techniques to protecting data stored in the cloud




Defensive Security in the Cloud
---------------------------------

This section focuses on how to discover risks early on, and what tools to use
to discover vulnerabilities.




Appendix
----------

- Authentication: Who can sign in and use the API.
- Authorization: What permissions that user has.