Getting your feet wet with [exoscale.ch](http://exoscale.ch)
============================================================

Start an instance via the UI
----------------------------
 
1. Go to [exoscale.ch](http://exoscale.ch) and click on `free sign-up` in the Open Cloud section
2. Ask me for the voucher code, you will get an additional 15CHF
2. Browse the UI, identify the `security groups` and `keypairs` sections.
3. Create a rule in your default security group to allow inbound traffic on port 22 (ssh)
4. Create a keypair and store the private key on your machine
5. Start an instance
6. ssh to the instance

Find your API Keys and inspect the API calls
--------------------------------------------

7. Inspect the API requests with firebug or dev console of your choice
8. Find your API keys (account section)

Get wordpress installed on an instance
---------------------------------------

Open port 80 on the default security group.
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

