.. meta::
    :description lang=en: Kubernetes
    :keywords: Kubernetes, kubectl, K8, clusters

============
Kubernetes
============

.. contents:: Table of Contents
    :backlinks: none

Introduction
--------------

Kubernetes is an open-source system for automating deployment, scaling, and management of containerized (ex: Docker) applications.

KUBECTL is a command line tool for running commands against a Kubernetes cluster.

Recommendation
---------------

KUBECTL communicates the clusters master system and can be used to interact with nodes, pods, and services

Use `EKS <aws-eks-basics.rst>`_ to create/delete/edit clusters


Read Cluster
-------------

.. code-block:: bash

    # check health of nodes and wait for them to reach the Ready status.
    $ kubectl get nodes --watch

