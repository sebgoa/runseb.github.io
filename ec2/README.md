AWS EC2 interface
=================

CloudStack has two Amazon WebServices EC2 interface, one built-in and one that was just released recently [ec2stack](https://github.com/BroganD1993/ec2stack).
These interfaces allow you to use any EC2 client to communicate with a CloudStack based cloud, they received EC2 api calls and forward them to 
a CloudStack cloud, mapping the EC2 calls to the appropriate CloudStack API.

In this section we are going to deploy `ec2stack` and test it using the [AWS CLI](http://aws.amazon.com/cli/)

Getting ready
-------------

We are going to use the Python package installer `pip`. If you haven't installed it yet on your machine, install it.
For example on a ubuntu machine:

    apt-get install -y python-pip
