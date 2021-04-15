.. meta::
    :description lang=en: AWS Identity and Access Management (IAM)
    :keywords: AWS, AWSCLI

==============================
IAM Configuration and Basics
==============================

.. contents:: Table of Contents
    :backlinks: none


IAM Dashboard: New Client
-------------------------

AWS Identity and Access Management (IAM) service allows you to authorize users / applications (such as AWS CLI) to access AWS resources.

Navigate into the AWS IAM Dashboard to create new user

.. code-block:: bash

    AWS IAM Dashboard -> Users -> Add User
    Permission: If unknown, set to AdministratorAccess policy. This will allow the new user to perform any action in your AWS account.


output:

.. code-block:: bash

    # WARNING: Credentials for new user will be downloadable once
    Username
    Password
    Access key ID
    Secret access key
    Console login link

AWS CLI: Configuration
-------------------------

Set new aws configuration

.. code-block:: bash

    $ aws configure --profile default
    # $ aws configure list # to see current config

output:

.. code-block:: bash

    AWS Access Key ID: ******
    AWS Secret Access Key: *****
    Default region name: us-east-2
    Default output format: json

See active users

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
                "Arn": "arn:aws:iam::********:user/new-user-1",
                "CreateDate": "*****************"
            }
        ]
    }
