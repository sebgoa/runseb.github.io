Install Vagrant and create the exo boxes
----------------------------------------

First install [Vagrant](http://www.vagrantup.com/downloads.html) and then get the cloudstack plugin:

    vagrant plugin install vagrant-cloudstack

Then we are going to clone a small github [project](https://github.com/exoscale/vagrant-exoscale-boxes) from exoscale. This project is going to give us *vagrant boxes*, fake virtual machine images that refer to Exoscale templates available.

    git clone https://github.com/exoscale/vagrant-exoscale-boxes
    cd vagrant-exoscale-boxes

Edit the `config.py` script to specify your API keys, then run:

    python ./make-boxes.py

If you are familiar with Vagrant this will be straightforward, if not, you need to add a box to your local installation for instance:

    vagrant box add Linux-Ubuntu-13.10-64-bit-50-GB-Disk /path/or/url/to/boxes/Linux-Ubuntu-13.10-64-bit-50-GB-Disk.box
