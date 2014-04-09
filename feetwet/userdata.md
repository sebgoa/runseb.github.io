Using UserData
--------------

Similarly to AWS, CloudStack supports `userdata`. We can therefore pass a script to the deploy virtual machine call.
Exoscale templates are setup with [cloudinit](https://help.ubuntu.com/community/CloudInit) which will parse the userdata and do what's required.

In this exericse we put a bash script in the UI user-data field and deploy `wordpress`.

Open port 80 on the default security group.

![Exoscale Security Group](../images/securitygroup.png)

Start an Ubuntu 12.04 instance and in the User-Data tab input:

    #!/bin/sh
    set -e -x

    apt-get --yes --quiet update
    apt-get --yes --quiet install git puppet-common

    #
    # Fetch puppet configuration from public git repository. 
    #

    mv /etc/puppet /etc/puppet.orig
    git clone https://github.com/retrack/exoscale-wordpress.git /etc/puppet
 
    #
    # Run puppet.
    #

    puppet apply /etc/puppet/manifests/init.pp

Now open your browser on port 80 of the IP of your instance and Voila !

