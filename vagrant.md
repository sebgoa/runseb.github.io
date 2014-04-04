Vagrant boxes
=============

Install Vagrant and create the exo boxes
----------------------------------------

[Vagrant](http://vagrantup.com) is a tool to create *lightweight, portable and reproducible development environments*. Specifically it allows you to use configuration management tools to configure a virtual machine locally (via virtualbox) and then deploy it *in the cloud* via Vagrant providers. 
In this next exercise we are going to install vagrant on our local machine and use Exoscale vagrant boxes to provision VM in the Cloud using configuration setup in Vagrant. For future reading check this [post](http://sebgoa.blogspot.co.uk/2013/12/veewee-vagrant-and-cloudstack.html)

First install [Vagrant](http://www.vagrantup.com/downloads.html) and then get the cloudstack plugin:

    vagrant plugin install vagrant-cloudstack

Then we are going to clone a small github [project](https://github.com/exoscale/vagrant-exoscale-boxes) from exoscale. This project is going to give us *vagrant boxes*, fake virtual machine images that refer to Exoscale templates available.

    git clone https://github.com/exoscale/vagrant-exoscale-boxes
    cd vagrant-exoscale-boxes

Edit the `config.py` script to specify your API keys, then run:

    python ./make-boxes.py

If you are familiar with Vagrant this will be straightforward, if not, you need to add a box to your local installation for instance:

    vagrant box add Linux-Ubuntu-13.10-64-bit-50-GB-Disk /path/or/url/to/boxes/Linux-Ubuntu-13.10-64-bit-50-GB-Disk.box

Initialize a `Vagrantfile` and start an instance
----------------------------------------------

Now you need to create a *Vagrantfile*. In the directory of you choice  for example `/tutorial` do:

    vagrant init

Then edit the `Vagrantfile` created to contain this:

    Vagrant.configure("2") do |config|
        config.vm.box = "Linux-Ubuntu-13.10-64-bit-50-GB-Disk"
        config.ssh.username = "root"
        config.ssh.private_key_path = "/Users/vagrant/.ssh/id_rsa.vagrant"

        config.vm.provider :cloudstack do |cloudstack, override|
            cloudstack.api_key = "AAAAAAAAAAAAAAAA-aaaaaaaaaaa"
            cloudstack.secret_key = "SSSSSSSSSSSSSSSS-ssssssssss"

            # Uncomment ONE of the following service offerings:
            cloudstack.service_offering_id = "71004023-bb72-4a97-b1e9-bc66dfce9470" # Micro - 512 MB
            #cloudstack.service_offering_id = "b6cd1ff5-3a2f-4e9d-a4d1-8988c1191fe8" # Tiny - 1GB
            #cloudstack.service_offering_id = "21624abb-764e-4def-81d7-9fc54b5957fb" # Small - 2GB
            #cloudstack.service_offering_id = "b6e9d1e8-89fc-4db3-aaa4-9b4c5b1d0844" # Medium - 4GB
            #cloudstack.service_offering_id = "c6f99499-7f59-4138-9427-a09db13af2bc" # Large - 8GB
            #cloudstack.service_offering_id = "350dc5ea-fe6d-42ba-b6c0-efb8b75617ad" # Extra-large - 16GB
            #cloudstack.service_offering_id = "a216b0d1-370f-4e21-a0eb-3dfc6302b564" # Huge - 32GB

            cloudstack.keypair = "vagrant" # for SSH boxes the name of the public key pushed to the machine
        end
    end

Make sure to set your API keys and your keypair properly. Also edit the `config.vm.box` line to set the name of the box you actually added with `vagrant box add` and edit the `config.ssh.private_key_path` to point to the private key you got from exoscale. In this configuration the default security group will be used.

You are now ready to bring the box up:

    vagrant up --provider=cloudstack

Don't forget to use the `--provider=cloudstack` or the box won't come up. Check the exoscale dashboard to see the machine boot, try to ssh into the box.

Add provisioning steps
----------------------

Once you have successfully started a machine with vagrant, you are ready to specify a provisioning script. Create a `boostrap.sh` bash script in your working directory and make it do whatever your want.

Add this provisioning step in the `Vagrantfile` like so:

    # Test bootstrap script
    config.vm.provision :shell, :path => "bootstrap.sh"

Relaunch the machine with `vagrant up` or `vagrant reload --provision`. To stop a machine `vagrant destroy`

You are now ready to dig deeper into Vagrant provisioning. See the provisioner [documentation](http://docs.vagrantup.com/v2/provisioning/index.html) and pick your favorite configuration management tool. For example with [chef](http://www.getchef.com) you would specify a cookbook like so:

    config.vm.provision "chef_solo" do |chef|
        chef.add_recipe "mycookbook"
    end

Puppet example
---------------

For [Puppet](http://docs.vagrantup.com/v2/provisioning/puppet_apply.html) remember the script that we put in the Userdata of the very first example. We are going to use the same Puppet configuration but via Vagrant.

Edit the `Vagrantfile` to have:

    config.vm.provision "puppet" do |puppet|
        puppet.module_path = "modules"
    end

Vagrant will look for the manifest in the `manifests` directory and for the modules in the `modules` directory.
Now simply clone the repository that we used earlier:

    git clone https://github.com/retrack/exoscale-wordpress

You should now see the `modules` and `manifests` directory in the root of your working directory that contains the `Vagrantfile`.
Remove the shell provisioning step, make sure to use the Ubuntu 12.04 template id and start the instance like before:

    vagrant up --provider=cloudstack

Open your browser and get back to Wordpress ! Of course the whole idea of Vagrant is that you can test all of these provisioning steps on your local machines using VirtualBox. Once you are happy with your recipes you can then move to provision in the cloud. Check out [Packer](http://packer.io) a related project which you can use to generate images for your cloud.

Playing with multi-machines configuration
-----------------------------------------

Vagrant is also very interesting because you can start multiple machines at [once](http://docs.vagrantup.com/v2/multi-machine/index.html). Edit the `Vagrantfile` to add a `web` and a `db` machine. Add the cloudstack specific information and specify different bootstrap scripts.

    config.vm.define "web" do |web|
      web.vm.box = "tutorial"
    end

    config.vm.define "db" do |db|
      db.vm.box = "tutorial"
    end

You can control each machine separately `vagrant up web`, `vagrant up db` or all at once in parallel `vagrant up`

Let the fun begin. Pick your favorite configuration management tool, decide what you want to provision, setup your recipes and launch the instances.
