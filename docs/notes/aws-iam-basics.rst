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

Create New User
------------------

- IAM Console:

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



AWS CLI Configuration
-----------------------

Use command line interface to set the AWS CLI configuration

.. code-block:: bash

    $ aws configure --profile default
    # $ aws configure list # to see current config

    # add info fetched from user
    AWS Access Key ID: ******
    AWS Secret Access Key: *****
    Default region name: us-east-2
    Default output format: json

Get list of users
------------------

- CLI:

    Use command line interface to see the list users

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

Get active user
------------------

.. code-block:: bash

    $ aws sts get-caller-identity

output:

.. code-block:: bash

    {
        "UserId": "**********************",
        "Account": "************",
        "Arn": "arn:aws:iam::<Account>:user/new-user-1"
    }
