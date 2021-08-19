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

    - Using IAM roles and IAM users
        To Provide Secure Access to Users or Applications by managing user identities within AWS. Roles are used to bridge the user to access the AWS resources

        - Authentication are defined through AWS IAM
        - Authorization (Permissions): IAM user and roles are similar.
            - IAM users: Each user is provided with keys that can be saved into a config file or code. Recommended only for Authentication
            - IAM roles: Credentials are temporary and there is no need to store API keys permanently. Recommended for Authorizations.
            - Ideally, user policy should assume a specific role. The specific role then manages the policies with the specific permissions to interact with AWS services.

        - The image below shows an example of an user getting access to AWS resources by attaching the policy permission directly to that user

        .. image:: images/IAM_diagram.png
           :width: 400

        - The image below shows an example of an user getting access to AWS resources by attaching the policy permission directly to the role. This means the user must assume the role to get access to AWS resources after logging in.

        - The image below shows an example of an user getting access into AWS with IAM user, and appropriate IAM role to access the appropriate cloud services

        .. image:: images/devOps_user_access.png
           :width: 400

        - The image below shows an applications that assume a role to get access to AWS resources.

        .. image:: images/developer_creating_app_access.png
           :width: 400

    - Using IAM roles and Identity Federation
        To Provide Secure Access to Users or Applications by managing user identities outside AWS. Roles are used to bridge the user to access the AWS resources

        - Identify Federation: Manages users identities using an external identify provider instead of managing it within AWS as shown on the previous section. The access is then directly mapped to IAM roles
        - Identify Providers provide RBAC (Role-based-access-control) grants user a temporary token/document used to authorize with AWS in order to get access to IAM roles, which then enables the user to access AWS resources.
            - SAML 2.0 Identify Providers: These are Cloud-based identify providers such as AWS SSO, OneLogin, Okta. They can also be Corporate active directory
            - Web Identify Providers: These can be Facebook, Google, Amazon, etc.


    - Least Privilege Permissions and Policies
        When creating IAM policies that provide permission to user and roles, we want to
        follow the common security practice of granting least privilege.

        - Least Privilege Access: Granting a user or application only the permissions they need to do the required task.


Securing Access to Cloud Infrastructure
--------------------------------------

This section focuses on technique to secure the cloud infrastructure such as
access to instances, servers, and cloud network.

    - Key Questions:
        - Which trusted networks will traffic to your cloud environment (subnets, servers, other infrastructure components) originate from?
        - Which components need network connectivity to other components? (Ex: a set of servers that need to connect to PORT 5432 database)
        - Are security groups and network ACLs specific as possible?
        - Which components take traffic from the internet? Can they be moved to private subnets?
        - How are servers managed? Are user managing the username and password themselves?

    - Techniques to Access Servers (instances) in the cloud: Not Recommended
        - Using key pair (private:public) to authenticate, and then SSH to connect to the instance. Risky since end user manages the private key
        - Enabling password in the instance to SSH. Risky since it leaves the server open to brute force attacks
        - Using Privileged Access Management (PAM) service coupled with Identify Provider removed managing the user credentials to a trusted process
        - Use System Manager (SSM) Service coupled with AWS IAM to obtain a session to the instance

    - Techniques to Access Servers (instances) in the cloud: Recommended - Immutable Instances
        - Once an instance is provisioned, the instance does not undergo any changes
        - Any changes that needs to be done (install, run app code) will be done on a preconfigured NEW virtual machine image.
        - The pre-configuration are security harding patches at the OS level.
        - The application team then uses the pre-configure image to add any additional configurations that the app needs, and updates the app
        - The image is then deployed as an instance into the environment. Once deployed, no changes can be made.
        - This method allows no individuals to log into an instance once it's been deployed

    - Techniques to Access Servers (instances) in the cloud: Recommended - Configuration Management Tool
        - Instance configuration and code are managed in a git repository
        - When the instance is deployed, then the configuration management tool enforces hardening and configuration policies
        - Configuration Management tools that do this are Ansible, Chef, Puppet

    - Methods to control and restrict network traffic to and from your cloud environment
        1. OS Based Firewalls which can be done by server admins and developers (windows firewalls and Linux IP Tables)
        2. Commercial firewall products can be deployed in the cloud too
        3. Controlling Access at subnet level and instance or resource level.
            - Layer 1: Network Access Control Lists (Network ACLs or NACLs) can be applied at a subnet level and allow you to add multiple firewall rules such as CIDR Block (IP allow or deny), I/O-bound traffic.
            - Layer 2: Security groups firewall construct in AWS which allows controls to be placed on individual resources that are deployed in a VPC.
        4. Controlling Access between private VPC networks to work locations or corporate owned network spaces.
            - VPN tunnels or dedicated network links
        - Controlling Egress Traffic can be done with
            - Internet gateways: Facilitates out-bound and in-bound traffic to the internet.
            - NAT gateways: Facilitates outbound only access. No inbound access allowed.
            - For both Security groups and NACLs, following the principle of the least privilege only allows traffic from specific hosts or IP ranges to specific ports.

Data Protection in the Cloud
-----------------------------

This section focuses on techniques to protecting data while in transit, and at rest when stored in the cloud

- Methods for Encrypting Application Data:
    - AWS Key Management Service (KMS): AWS KMS generates (stores and manages) the encryption key to use when encrypting the data
    - Client-Side Encryption: Encrypting the data prior to storing the data in memory disk or cloud storage
    - Server-Side Encryption: Encrypting the data is handled by AWS storage service.

- Examples
    - Client-Side Encryption: Application code fetches AWS KMS to get key, and then encrypts using a "language specific encryption library"
    - Client-Side Encryption: Application code fetches AWS KMS to get key, and then encrypts using AWS Encryption SDK.
    - Server-Side Encryption: EBS or EFS volumes (disk storage in AWS) need to be configured to use KMS encryption
    - Server-Side Encryption: Amazon RDS (database storage in AWS) need to be configured to use KMS encryption
    - Server-Side Encryption: S3 bucket encryption is enabled so data is encrypted
        - S3-Managed Keys: S3 handles the entire encryption process
        - AWS-Managed Master Key: AWS manages the master key through KMS for the S3 service
        - Customer-Managed Master Key: User defines which master key to use in KMS, and also needs permission to access the key for the S3 service
        - Customer-Provided Keys: User provides the key for the S3 service to use.
- Continue to apply the principle of least privilege.
    - 'Customer-Managed Key in KMS allow us to specify the IAM user or roles to manage keys.
    - 'Customer-Managed Key in KMS allow us to specify which IAM user or roles can use the keys
    - 'Customer-Managed Master Key': Restrict specific AWS services to be allowed to use the key. For example we can restrict a key to be only used for decrypting S3 objects by application ABC however application will not be able perform decryption actions on DynamoDB.



Defensive Security in the Cloud
---------------------------------

This section focuses on how to discover risks early on, and what tools to use to discover vulnerabilities.

- Understanding the threat landscape (infrastructure)
    - Perform risk analysis to quantify specific vulnerabilities on risk and exploitations on Identity and Permission, Network and Infrastructure, and Data.
    - Run static code analysis (Open Policy Agent/Regula) on SaaC such as Terraform to check if a configuration is out of compliance with security standards.

- Identifying Misconfigurations and Vulnerabilities
    - These are processes that assess each part of the application stack policies/configurations (code to core cloud infrastructure services)
    - Cloud infrastructure Services:
        - AWS tools such as 'AWS Config' can provide snapshot insights of a particular resource (what changed, when it changed, how it was configured)
        - AWS Security Hub aggregates all AWS security monitoring services such as AWS Config, Inspector, GuardDuty into a single pane of glass.
        - Third party tools can also be integrated into AWS account to monitor the security and compliance.
    - Operating System Vulnerabilities:
        - AWS Inspector can identify vulnerabilities at the OS level for EC2 instances. However, it only provides static scanning.
        - Third-party vendors can provide dynamic or static scanning capabilities.

- Identify Suspicious Activity
    - The use of implementing the right tools for threat defense and monitoring. These can be
        - AWS CloudTrail: Logs activities within an AWS account. (Recommended to link it to a central S3 bucket)
        - VPC Flow Logs: Provides logs into network activities such as connection attempts/rejects.
        - S3 Bucket Logs: Logs any attempts to read or write objects to S3 buckets. All logs appear in CloudTrail.
        - AWS Config logs: Logs any state changes for any resources that are been monitored by AWS Config.





Appendix
----------

- Authentication: Who can sign in and use the API.
- Authorization: What permissions that user has.
- Key Pair: A key pair consists of a public key and a private key that authenticate and encrypt an SSH session. The public key can be hosted on cloud servers and the private key is held by the user. Only a user with the private key that corresponds to the public key will be able to authenticate to the SSH server.
- Privilege Access Management (PAM) Tool: A privileged access management tool provides management of authentication, sessions, password storage, audit, and privilege escalation when it comes to logging into server infrastructure.
- AWS Systems Manager (SSM): The AWS Systems Manager service provides the ability to manage EC2 instances at the operating system level - including patching, command execution, automation, state management, inventory.
- Ingress (inbound): In-bound network traffic that is entering your cloud environment from the outside. (ex: Incoming web application traffic)
- Egress (outbound): Out-bound network traffic that is leaving your cloud environment. (ex: Integrations to read from other cloud based APIs, sending metric to monitoring tools)
