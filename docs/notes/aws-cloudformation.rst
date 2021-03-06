.. meta::
    :description lang=en: AWS Cloud Formation
    :keywords: CloudFormation, Cloud Formation, AWS, Stack

===============
CloudFormation
===============

.. contents:: Table of Contents
    :backlinks: none

Introduction
--------------

CloudFormation is a service for managing the creation of Amazon resources. Resources created together are grouped in a stack.

Stack: It is a logical group of related resources on AWS. It is managed as a single entity.

Like any other AWS service, CloudFormation can be used via either the CLI or web-console.

Create template
-----------------

CloudFormation stacks are defined in template files, (.yml). These files define all of the resources for your stack (the collection of AWS resources that you want to create in just one command.)

Ex: myFirstTemplate.yml type 'AWS::EC2::VPC' is a virtual network defined by a range of IP addresses allocated to you. These IP addresses are by-default private to you, meaning no one will be able to access these IP addresses from the outside-world. You can associate any of these IP addresses to the resources, such as Database, server, load-balancer, cluster etc.

    .. code-block:: yaml

        AWSTemplateFormatVersion: 2010-09-09
        Description: Udacity - This template deploys a VPC
        Resources:
          myUdacityVPC:
            Type: 'AWS::EC2::VPC'
            Properties:
              CidrBlock: 10.0.0.0/16
              EnableDnsHostnames: 'true'

Create Stack
-------------

Creates a new stack

.. code-block:: bash

    $ aws cloudformation create-stack  --stack-name myFirstTest --region us-east-2 --template-body file://myFirstTemplate.yml


Update Stack
-------------

Updates an existing stack

.. code-block:: bash

    $ aws cloudformation update-stack  --stack-name myFirstTest --region us-east-2 --template-body file://myFirstTemplate.yml


Describe Stack
---------------

Describes properties of an existing stack

.. code-block:: bash

    $ aws cloudformation describe-stacks --stack-name myFirstTest


Delete Stack
--------------

Deletes an existing stack

.. code-block:: bash

    $ aws cloudformation delete-stack --stack-name myFirstTest