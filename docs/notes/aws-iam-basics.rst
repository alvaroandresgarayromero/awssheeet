.. meta::
    :description lang=en: AWS Identity and Access Management (IAM)
    :keywords: AWS, AWSCLI

=============
IAM
=============

.. contents:: Table of Contents
    :backlinks: none

Introduction
-------------
AWS Identity and Access Management (IAM) service allows you to authorize users / applications (such as AWS CLI) to access AWS resources.

User
-----

- Create new user (w/ IAM Console):

    Navigate into the AWS IAM Console to create new user

    .. code-block:: bash

        AWS IAM Dashboard -> Users -> Add User
        Permission: If unknown, set to AdministratorAccess policy. An IAM policy is a JSON file that defines the level of permissions (authorization) a user (or a service) can have while accessing AWS services in your account. This will allow the new user to perform any action in your AWS account.


        # Credentials for new user will be created (see below) and downloadable as CSV "ONCE"
        Username
        Password
        Access key ID
        Secret access key
        Console login link


- Get list of users

    CLI: Use command line interface to see the list users

    .. code-block:: bash

        $ aws iam list-users

    output:

    .. code-block:: bash

        {
            "Users": [
                {
                    "Path": "/",
                    "UserName": "new-user-1",
                    "UserId": "******************",
                    "Arn": "arn:aws:iam::<Account>:user/new-user-1",
                    "CreateDate": "*****************"
                }
            ]
        }

- Get active user

    CLI: Use command line interface to see the list users

    .. code-block:: bash

        $ aws sts get-caller-identity

    output:

    .. code-block:: bash

        {
            "UserId": "**********************",
            "Account": "************",
            "Arn": "arn:aws:iam::<Account>:user/new-user-1"
        }


Roles
------
IAM roles are a secure way to grant permissions to entities that you trust.

Permission Policy: The first is the account that owns the resource (the trusting account).
Trusted Policy: The second is the account that contains the users that need to access the resource (the trusted account).

- Permission Policy:

    What resources can be accessed and what actions can be taken

    .. code-block:: bash

        # example: access the description of the EKS cluster
        #          and fetch a list of necessary parameters
        #          from the AWS Systems Manager service
        {
        "Version": "2012-10-17",
        "Statement": [
          {
              "Effect": "Allow",
              "Action": [
                  "eks:Describe*",
                  "ssm:GetParameters"
              ],
              "Resource": "*"
          }
        ]
        }


- Trusted Policy:

    What entities can assume the role

    .. code-block:: bash

        # trust.json

        {
        "Version": "2012-10-17",
        "Statement": [
         {
             "Effect": "Allow",
             "Principal": {
                 "AWS": "arn:aws:iam::<ACCOUNT_ID>:root"
             },
             "Action": "sts:AssumeRole"
         }
        ]
        }

    .. code-block:: bash

        # create the role
        $ aws iam create-role --role-name UdacityFlaskDeployCBKubectlRole \
                              --assume-role-policy-document file://trust.json \
                              --output text --query 'Role.Arn



AWS CLI Configuration
-----------------------

Use command line interface to set the AWS CLI configuration

.. code-block:: bash

    $ aws configure --profile default
    # $ aws configure list # to see current config

    # info can be found from created user
    AWS Access Key ID: ******
    AWS Secret Access Key: *****
    Default region name: us-east-2
    Default output format: json

