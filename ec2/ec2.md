AWS EC2 interface
=================

CloudStack has two Amazon WebServices EC2 interface, one built-in and one that was just released recently [ec2stack]().
These interfaces allow you to use any EC2 client to communicate with a CloudStack based cloud, they received EC2 api calls and forward them to 
a CloudStack cloud, mapping the EC2 calls to the appropriate CloudStack API.

In this section we are going to deploy ec2stack and test it using the [AWS CLI]()

Getting ready
-------------

We are going to use the Python package installer `pip`. If you don't have it on your machine, install it.
For example on a ubuntu machine:

    apt-get install -y python-pip

Install aws cli
---------------

Simply use pip

    pip install awscli

A configuration file is placed in `~/.aws/config` later on we will configure this CLI.

Install ec2stack
----------------

Using pip, it is a single operation.

    pip install ec2stack

If you want to do it from source and check out the code, then clone the git repository:

    git clone https://github.com/imduffy15/ec2stack.git

Then install it with

    sudo python ./setup.py install

Configure ec2stack
------------------

`ec2stack` can be configured with `ec2stack-configure` command. To set it up with [exoscale](http://exoscale.ch) do:

    $ec2stack-configure
    EC2Stack bind address [0.0.0.0]: 
    EC2Stack bind port [5000]: 
    Cloudstack host [localhost]: api.exoscale.ch
    Cloudstack port [8080]: 443
    Cloudstack protocol [http]: https
    Cloudstack path [/client/api]: /compute
    Cloudstack custom disk offering name [Custom]: 
    Cloudstack default zone name: CH-GV2
    Do you wish to input instance type mappings? (Yes/No): Yes
    Insert the AWS EC2 instance type you wish to map: m1.small
    Insert the name of the instance type you wish to map this to: Tiny
    Do you wish to add more mappings? (Yes/No): No
    INFO  [alembic.migration] Context impl SQLiteImpl.
    INFO  [alembic.migration] Will assume non-transactional DDL.

Note that we created a mapping between the AWS m1.small instance type to the `Tiny` instance type in exoscale.
You could add more mappings.

Run ec2stack
------------

Once you configured it, run the `ec2stack` server:

    $ec2stack

By default it will run on localhost and port 5000

Register a user
---------------

To be on the safe side, we are going to upgrade the `requests` module

    $pip install --upgrade requests

Now you need to register your API keys from exoscale

    $ec2stack-register http://localhost:5000 <your API accesskey> <your API secret key>

The command should return a `Successfully Registered!` message.

In addition you need to register your exoscale API keys with the `aws` CLI:

    $aws configure
    AWS Access Key ID [None]: PQogHs2sk_3uslfvrASjQFDlZbt0mEDd14iNSgCsgB0DoCjAFakosJINePC4JsFiyHzQ3LwlWIXZbVdNPUn-mg
    AWS Secret Access Key [None]: aHuDB2ewpgxVuQlvD9P1o313BioI1W4vFCxQpTCqGvbqj3Y6mVZo-paBbYyE3W_TQKEirnTKENNRC5NR5cUjEg
    Default region name [None]: CH-GV2
    Default output format [None]:

    $aws configure set default.ec2.signature_version v2

You can see these settings in the `~/.aws/config` mentioned earlier.
Check the AWS CLI [reference](http://docs.aws.amazon.com/cli/latest/reference/) for further customization.
The output format can be `json`, `text` or `table`.

You are now ready to try the AWS interface

Using the AWS interface `ec2stack`
----------------------------------

It is almost the same as using `aws` with Amazon, the only thing that changes is a new endpoint, try to list the availability zones:

    $aws ec2 describe-availability-zones --endpoint=http://localhost:5000
    -----------------------------------------
    |       DescribeAvailabilityZones       |
    +---------------------------------------+
    ||          AvailabilityZones          ||
    |+------------+-----------+------------+|
    || RegionName |   State   | ZoneName   ||
    |+------------+-----------+------------+|
    ||  CH-GV2    |  Enabled  |  CH-GV2    ||
    |+------------+-----------+------------+|

You can now explore the available api [calls](https://github.com/BroganD1993/ec2stack/blob/master/ec2stack/controllers/default.py).

To list the images available:

    $aws ec2 describe-images --endpoint=http://localhost:5000

To create a key pair:

    $aws ec2 create-key-pair --endpoint=http://localhost:5000 --key-name=test

If you want to use it don't forget to get the private key from the output and put it in your `~/.ssh/` directory.

To delete the key pair:

    $aws ec2 delete-key-pair --endpoint=http://localhost:5000 --key-name=test

Finally the holly grail of IaaS, let's start an instance:

    $aws ec2 run-instances --image-id=20d4ebc3-8898-431c-939e-adbcf203acec --endpoint=http://localhost:5000

The image-id parameter is the CloudStack uuid corresponding to the template that you want to start. You find it by running the aws `describe-images` call.

You can now list the instances:

    $aws ec2 describe-instances --endpoint=http://localhost:5000

And terminate them:

    $aws ec2 terminate-instances --instance-ids=259851ae-281b-45cf-bf1c-df83e3a885e1 --endpoint=http://localhost:5000

That's it, have fun and contribute more API mappings to `ec2stack`

