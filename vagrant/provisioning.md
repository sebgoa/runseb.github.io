dd provisioning steps
----------------------

Once you have successfully started a machine with vagrant, you are ready to specify a provisioning script. Create a `boostrap.sh` bash script in your working directory and make it do whatever you want.

Add this provisioning step in the `Vagrantfile` like so:

    # Test bootstrap script
    config.vm.provision :shell, :path => "bootstrap.sh"

Relaunch the machine with `vagrant up` or `vagrant reload --provision`. To stop a machine `vagrant destroy`

You are now ready to dig deeper into Vagrant provisioning. See the provisioner [documentation](http://docs.vagrantup.com/v2/provisioning/index.html) and pick your favorite configuration management tool. For example with [chef](http://www.getchef.com) you would specify a cookbook like so:

    config.vm.provision "chef_solo" do |chef|
        chef.add_recipe "mycookbook"
    end

In the next section we will do a `puppet` example, however if you know [chef](http://www.getchef.com) or [salt](http://www.saltstack.com) you can do an example with those.
