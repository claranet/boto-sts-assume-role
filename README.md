# botosts - AWS Python SDK wrapper
[![boto3](https://img.shields.io/badge/documentation-boto3-brightgreen.svg)](https://boto3.readthedocs.io/en/latest/) [![boto2](http://img.shields.io/badge/documentation-boto-brightgreen.svg)](http://boto.cloudhackers.com/en/latest/ref/) [![Changelog](https://img.shields.io/badge/changelog-release-green.svg)](https://github.com/claranet/boto-sts-assume-role/blob/master/CHANGELOG.md) [![Apache V2 License](http://img.shields.io/badge/license-Apache%20V2-blue.svg)](https://github.com/claranet/boto-sts-assume-role/blob/master/LICENSE)

botosts is a wrapper on top of boto(2) and boto3.
It handles STS Assume Role and return the appropriated boto2 or boto3 client, fully initialized.

Key features
------------

* Compatible with boto2 and boto3
* Can handle a standard client (using boto default credentials) or can STS Assume Role to another IAM Role account.
* Compatible with all AWS Partitions: AWS Standard, AWS China, AWS US Gov
* Uses heuristic endpoint option which allows to have latest AWS Regions (Paris, London, etc.) available with a boto2 client.

Requirements
------------

* Python 2.7+

Installation
------------

With [pip](https://pip.pypa.io) (in a [virtualenv](https://virtualenv.pypa.io) or not)

```
pip install git+ssh://bitbucket.org/morea/boto-sts-assume-role
```

Examples
--------

A boto2 client, with standard credentials (current IAM profile), connected to S3:

```python
from botosts.aws_connection import AWSConnection

cloud_connection = AWSConnection()
client = cloud_connection.get_connection(region, ["s3"])
```

A boto3 client, with an assume role for another account, connected to EC2 autoscale:

```python
from botosts.aws_connection import AWSConnection

cloud_connection = AWSConnection(
    assumed_account_id=args.assumed_account_id,
    assumed_role_name=args.assumed_role_name,
    assumed_region_name=args.region
)
client = cloud_connection.get_connection(region, ['autoscaling'], boto_version='boto3')
```

A boto2 client, in AWS China, connected to EC2 ELB:
```python
from botosts.aws_connection import AWSConnection
cloud_connection = AWSConnection(
    config={'aws_partitions': ['aws-cn']}
)
client = cloud_connection.get_connection('cn-north-1', ['ec2', 'elb'], boto_version='boto2')
```

License
-------

This SDK is distributed under the
[Apache License, Version 2.0](http://www.apache.org/licenses/LICENSE-2.0),
see [LICENSE](https://github.com/claranet/boto-sts-assume-role/blob/master/LICENSE) and [NOTICE](https://github.com/claranet/boto-sts-assume-role/blob/master/NOTICE) for more information.
