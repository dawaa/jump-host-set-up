How to set up a Jump Host
============================

## Table of Contents
* [Create your EC2 Container](create-your-ec2-container)
* [Install AWS CLI](install-aws-cli)
    * [Configure](configure-aws-cli)
* [Create / Manage users](create--manage-users-for-the-instances-within-vpc)
* [SSH via JumpHost](ssh-via-jumphost)
* [Helpful sources](helpful-sources)

## Create your EC2 Container

## Install "aws-cli"
Firstly we must grab the awscli bundle from amazon to our newly created
ec2 instance..
> Make sure Python is installed...
> Make sure you have `zip` and `unzip` packages installed

`$ curl "https://s3.amazonaws.com/aws-cli/awscli-bundle.zip" -o "awscli-bundle.zip"`


Unzip the bundle...

`$ unzip awscli-bundle.zip`

Install it...

`$ ./awscli-bundle/install -i /usr/local/aws -b /usr/local/bin/aws`


|  Parameter  |  Purpose                    |
|-------------|-----------------------------|
| -i          | Installation directory      |
| -b          | Binary Location for AWS CLI |


Test it...

`$ aws --version`


## Configure AWS CLI
Go here to get your `AWS Access Key ID` and `AWS Secret Access Key` from the
AWS Management Console from the root user `Security Credentials`.

Click "Create New Access Key" and copy the values presented.

https://console.aws.amazon.com/iam/home?#/security_credential

`$ aws configure`


## Create / Manage users for the instances within VPC
To create a new user and allow them ssh access to the private network via the
jump host we must first make sure the user(s) exist on the server..

To create a user
```
$ sudo su (unless you are root-user already)
$ useradd -c "Name Lastname" <username>
$ cd /home/<username>
$ mkdir .ssh
$ chmod 700 .ssh
$ chown <username>:<username> .ssh
$ touch .ssh/authorized_keys
# Now add their pub-key content to "authorized_keys"

$ chmod 600 .ssh/authorized_keys
$ chown <username>:<username> .ssh/authorized_keys
```


## SSH via JumpHost
To enable SSH via the JumpHost to the VPC it exposes we must make sure that
the following has been done..

```
$ ssh-add [-l|-L]
```
Should output the .pem key created when creating the EC2 instance and also
the .pub key you want to use to prove yourself to be you.


Example SSH
```
$ ssh -A ec2-user@ec2-34-212-80-162.us-west-2.compute.amazonaws.com
# The -A flag means Agent Forwarding which is required to ensure higher
# security

> ec2-user@ip-172-31-28-214 ~]
$ ssh alle@172.31.23.94 # This is the private IP of the instance
> alle@ip-172-31-23-94 ~]
# Successfully SSH'd in to our EC2 container via the jumphost
```


## Helpful-sources
- https://www.slideshare.net/mvcp007/how-to-install-and-configure-aws-cli-on-rhel-7-57163940
- https://console.aws.amazon.com/iam/home?#/security_credential
- http://awscli.com/get-a-list-of-instance-with-id-name-and-type.html
- https://aws.amazon.com/blogs/security/securely-connect-to-linux-instances-running-in-a-private-amazon-vpc/
- http://www.ampedupdesigns.com/blog/show?bid=44
- https://aws.amazon.com/items/1233?externalID=1233
