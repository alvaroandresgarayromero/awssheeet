.. meta::
    :description lang=en: Amazon EKS
    :keywords: AWS, EKS, Elastic Kubernetes Service


=========================================
Elastic Kubernetes Service (Amazon EKS)
=========================================

.. contents:: Table of Contents
    :backlinks: none

Introduction
--------------
EKSCTL greatly simplifies EKS cluster creation, by auto-generating the necessary AWS resources in your default region

The EKSCTL utility eliminates the need to write a CloudFormation script. In the case of an advanced cluster with desired configuration, you may have to write a minimal YAML script.

AWS CloudFormation is an AWS service for creating, managing, and configuring ANY resource on the AWS cloud using a YAML/JSON script. In the script file, you can define the properties of the resource you want to create in the cloud.

Recommendation
---------------

Use EKSCTL to create/edit/delete EKS clusters

`Kubernetes (kubectl) <kubernetes-basics.rst>`_ communicates the clusters master system and can be used to interact with nodes, pods, and services

Create Cluster
------------------

Go to the CloudFormation console to view the progress.

.. code-block:: bash

    # use default parameters
    $ eksctl create cluster

.. code-block:: bash

    # explicitly define
    $ eksctl create cluster --name myCluster --region=us-east-2

.. code-block:: bash

    # explicitly define
    $ eksctl create cluster --name myCluster --nodes=4

.. code-block:: bash

    # define config details (YAML file)
    $ eksctl create cluster --config-file=<path>

Read Cluster
------------------

.. code-block:: bash

    # use default parameters
    $ eksctl get cluster [--name=<name>][--region=<region>]


Delete Cluster
------------------

.. code-block:: bash

    # use default parameters
    $ eksctl delete cluster --name=<name> [--region=<region>]

