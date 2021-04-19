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

Allowing AWS users/services to the cluster
------------------------------------------------

Kubernetes maintains a file, ConfigMap, which grants additional AWS users or roles the ability to interact with your cluster. The framework that manages access is known as the Kubernetes RBAC.

Role-based access control (RBAC) is a method of regulating access to computer or network resources based on the roles of individual users within your organization.

Note: In AWS EKS, when you create an Amazon EKS cluster, then only YOU have the permission to administer a newly created cluster. No other user/AWS service will be able to perform any action. To grant others access, then the ConfigMap needs to be updated.

- Fetch ConfigMap:

.. code-block:: bash

    kubectl get -n kube-system configmap/aws-auth -o yaml > /tmp/aws-auth-patch.yml

- Edit ConfigMap: Add role (ex:UdacityFlaskDeployCBKubectlRole) into ConfigMap

    - Role:

    .. code-block:: yaml

        - groups:
          - system:masters
          rolearn: arn:aws:iam::<ACCOUNT_ID>:role/UdacityFlaskDeployCBKubectlRole
          username: build

    - Before ConfigMap

    .. code-block:: yaml

        apiVersion: v1
        data:
          mapRoles: |
            - groups:
              - system:bootstrappers
              - system:nodes
              rolearn: arn:aws:iam::<ACCOUNT_ID>:role/eksctl-myCluster-nodegroup-ng-4ba-NodeInstanceRole-1GQUA77OEL85J
              username: system:node:{{EC2PrivateDNSName}}
          mapUsers: |
            []
        kind: ConfigMap
        metadata:
          creationTimestamp: "2021-04-18T15:26:02Z"
          name: aws-auth
          namespace: kube-system
          resourceVersion: "1400"
          selfLink: /api/v1/namespaces/kube-system/configmaps/aws-auth
          uid: 28d1c7e3-2399-4e4c-9ffa-6feeca86051b

    - After ConfigMap

    .. code-block:: yaml

        apiVersion: v1
        data:
          mapRoles: |
            - groups:
              - system:masters
              rolearn: arn:aws:iam::<ACCOUNT_ID>:role/UdacityFlaskDeployCBKubectlRole
              username: build
            - groups:
              - system:bootstrappers
              - system:nodes
              rolearn: arn:aws:iam::<ACCOUNT_ID>:role/eksctl-myCluster-nodegroup-ng-4ba-NodeInstanceRole-1GQUA77OEL85J
              username: system:node:{{EC2PrivateDNSName}}
          mapUsers: |
            []
        kind: ConfigMap
        metadata:
          creationTimestamp: "2021-04-18T15:26:02Z"
          name: aws-auth
          namespace: kube-system
          resourceVersion: "1400"
          selfLink: /api/v1/namespaces/kube-system/configmaps/aws-auth
          uid: 28d1c7e3-2399-4e4c-9ffa-6feeca86051b

- Update

.. code-block:: bash

    $ kubectl patch configmap/aws-auth -n kube-system --patch "$(cat /tmp/aws-auth-patch.yml)"

    # output: configmap/aws-auth patched
