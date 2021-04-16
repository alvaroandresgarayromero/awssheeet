.. meta::
    :description lang=en: Amazon S3 Bucket
    :keywords: AWS, AWSCLI


===========
Amazon S3
===========

.. contents:: Table of Contents
    :backlinks: none

Introduction
--------------
Amazon Simple Storage Service (Amazon S3) is an object storage service that offers industry-leading scalability, data availability, security, and performance

Create New Bucket
-------------------

- S3 Bucket Dashboard:

    Navigate into the AWS S3 Console to create a new bucket

.. code-block:: bash

    Amazon S3 -> Buckets -> Create Buckets

- S3 Bucket CLI:

    Use command line interface to create a new bucket

.. code-block:: bash

    #  Regions outside of us-east-1 require the appropriate LocationConstraint to be specified in order to create the bucket in the desired region
    $ aws s3api  create-bucket --bucket mybucketname --acl public-read-write --region us-east-2 --create-bucket-configuration LocationConstraint=us-east-2

Push file to Bucket
-------------------

.. code-block:: bash

    $ echo "this is a sample file" > sample.txt
    $ aws s3api put-object --bucket mybucketname --key sample.txt --body sample.txt

Delete file from Bucket
------------------------

.. code-block:: bash

    $ aws s3api delete-object --bucket mybucketname --key sample.txt

Delete Bucket
------------------------

.. code-block:: bash

    $ aws s3api delete-bucket --bucket mybucketname