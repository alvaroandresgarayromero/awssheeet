.. meta::
    :description lang=en: Infrastructure as a Code
    :keywords: AWS, AWSCLI


=========================
Infrastructure as a Code
=========================

.. contents:: Table of Contents
    :backlinks: none

Definition
------------

The use of code to define your infrastructure.
This method reduces human error, and increases
automation and collaboration.


Terraform
-----------
- Cloud neutral IaC (supports AWS, GCP, Azure, Digital Ocean)

- Workflow begins with a terraform (TF) config file. TF then executes based on the next states
    - Refresh - TF fetches the real-world (current state of infrastructure)
    - Plan - TF plans what needs to do done to achieve the new desired infrastructure config
    - Apply - TF applies infrastructure to the real world
    - Destroy (final day): TF removes infrastructure from the real world

- TF main components:
    - Core
        - input: reads TF config, and state (figures out what needs to created, updated, deleted)
        - output: supports public cloud providers (AWs, GCP, etc), platform as a service (heroku, kubernetes, lambdas), Software as a Service (Github)

- .tf is TF config file
- .tfvars is a input variables file (similar to environment variables), which can be used to modify modules
- TF modules abstract the infrastructure config. These are a set of TF config files in a folder.


Collaboration and Security
---------------------------
Manage terraform.tfstate with a remote backend

- .tfstate is a terraform generated file that tracks the state of the infrastructure.
    - This file is stored locally by default or can be configure to be remotely source controlled on s3, github, terraform pro by defining backend.tf




IaC Best Practices
-------------------


