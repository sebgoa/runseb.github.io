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


