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

