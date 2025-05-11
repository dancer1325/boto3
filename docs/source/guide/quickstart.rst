.. _guide_quickstart:

Quickstart
==========

* goal
    * install or update -- the -- AWS SDK for Python

* AWS SDK for Python
    * == Botocore + Boto3
        * Botocore
            * == Python package (library)
            * provide
                * low-level functionality / shared BETWEEN Python SDK -- & -- AWS CLI
        * Boto3
            * == package /
                * implement the Python SDK itself

Installation
------------

* install Boto3 + its dependencies

.. _quickstart_install_python:

Install or update Python
~~~~~~~~~~~~~~~~~~~~~~~~

* install Python v3.9+
    * Reason: 🧠Python v3.8- is deprecated🧠
    * if you need to upgrade -> see :ref:`guide_migration_py3`

Setup a virtual environment
~~~~~~~~~~~~~~~~~~~~~~~~~~~

* provides
    * isolated space | your installation
* steps::

    $ python -m venv .venv
    ...
    $ source .venv/bin/activate


* Reason: 🧠avoid
    * unexpected dependency conflicts OR
    * failures -- with -- other tools installed | your system🧠

Install Boto3
~~~~~~~~~~~~~

::

    pip install boto3

* if your project requires a specific version -> ::

    # Install Boto3 version 1.0 specifically
    pip install boto3==1.0.0

    # Make sure Boto3 is no older than version 1.15.0
    pip install boto3>=1.15.0

    # Avoid versions of Boto3 newer than version 1.15.3
    pip install boto3<=1.15.3

Using the AWS Common Runtime (CRT)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

* TODO: In addition to the default install of Boto3, you can choose to include the new `AWS Common Runtime <https://docs.aws.amazon.com/sdkref/latest/guide/common-runtime.html>`_
(CRT). The AWS CRT is a collection of modular packages that serve as a new foundation for AWS SDKs.
Each library provides better performance and minimal footprint for the functional area it
implements. Using the CRT, SDKs can share the same base code when possible, improving consistency
and throughput optimizations across AWS SDKs.

When the AWS CRT is included, Boto3 uses it to incorporate features not otherwise
available in the AWS SDK for Python.

You'll find it used in features like:

-  `Amazon S3 Multi-Region Access Points <https://docs.aws.amazon.com/AmazonS3/latest/userguide/MultiRegionAccessPoints.html>`_
-  `Amazon S3 Object Integrity <https://docs.aws.amazon.com/AmazonS3/latest/userguide/checking-object-integrity.html>`_
-  Amazon EventBridge Global Endpoints

However, Boto3 doesn't use the AWS CRT by default but you can opt into using it by specifying the
:code:`crt` `extra feature <https://www.python.org/dev/peps/pep-0508/#extras>`_ when installing Boto3::

    pip install boto3[crt]

To revert to the non-CRT version of Boto3, use this command::

    pip uninstall awscrt

If you need to re-enable CRT,  reinstall :code:`boto3[crt]` to ensure you get a compatible version of :code:`awscrt`::

    pip install boto3[crt]

Configuration
-------------

Before using Boto3, you need to set up authentication credentials for your AWS account using either
the `IAM Console <https://console.aws.amazon.com/iam/home>`_ or the AWS CLI. You can either choose
an existing user or create a new one.

For instructions about how to create a user using the IAM Console, see `Creating IAM users
<https://docs.aws.amazon.com/IAM/latest/UserGuide/id_users_create.html#id_users_create_console>`_.
Once the user has been created, see `Managing access keys
<https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_access-keys.html#Using_CreateAccessKey>`_
to learn how to create and retrieve the keys used to authenticate the user.

If you have the `AWS CLI <http://aws.amazon.com/cli/>`_ installed, then you can use the
:command:`aws configure` command to configure your credentials file::

    aws configure

Alternatively, you can create the credentials file yourself. By default, its location is
``~/.aws/credentials``. At a minimum, the credentials file should specify the access key and secret
access key. In this example, the key and secret key for the account are specified in the ``default`` profile::

    [default]
    aws_access_key_id = YOUR_ACCESS_KEY
    aws_secret_access_key = YOUR_SECRET_KEY

You may also want to add a default region to the AWS configuration file, which is located by default
at ``~/.aws/config``::

    [default]
    region=us-east-1

Alternatively, you can pass a ``region_name`` when creating clients and resources.

You have now configured credentials for the default profile as well as a default region to use when
creating connections. See :ref:`guide_configuration` for in-depth configuration sources and options.

Using Boto3
------------

::

    import boto3

    # indicate AWS server / you are going to use
    s3 = boto3.resource('s3')

    # Print out bucket names
    for bucket in s3.buckets.all():
        print(bucket.name)

    # Upload a NEW file | EXISTING "amzn-s3-demo-bucket" S3 bucket
    with open('test.jpg', 'rb') as data:
        s3.Bucket('amzn-s3-demo-bucket').put_object(Key='test.jpg', Body=data)

* see
    * :ref:`guide_resources`
    * :ref:`guide_collections`
