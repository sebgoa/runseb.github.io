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

