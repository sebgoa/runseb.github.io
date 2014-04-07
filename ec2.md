AWS EC2 interface
=================

CloudStack has two Amazon WebServices EC2 interface, one built-in and one that was just released recently [ec2stack]().
These interfaces allow you to use any EC2 client to communicate with a CloudStack based cloud, they received EC2 api calls and forward them to 
a CloudStack cloud, mapping the EC2 calls to the appropriate CloudStack API.

In this section we are going to deploy ec2stack and test it using the [AWS CLI]()

Install aws cli
---------------

Simply use pip

    pip install awscli

A configuration file is placed in `~/.aws/config` if you have an AWS EC2 account you can configure this file right away by running

   aws configure


Install ec2stack
----------------

Using pip

    pip install ec2stack

If you want to do it from source and check out the code, then clone the git repository:

    git clone https://github.com/imduffy15/ec2stack.git

Install with

    sudo python ./setup.py install


Run ec2stack
------------

Once you configured it, run the `ec2stack` server:

    $ec2stack

By default it will run on localhost and port 5000



