Provisioning with Playbooks
----------------------------

Clone the `ansible-examples` outside your Vagrant project:

    cd ..
    git clone https://github.com/ansible/ansible-examples.git

Pick the one you want to try, go easy first :) Maybe wordpress or a lamp stack and copy it's content to a `ansible` directory within the root of the Vagrant project.

    cd ./tutorial
    mkdir ansible
    cd ansible
    cp -R ../../ansible-examples/wordpress-nginx/ .

Go back to the Vagrant project directory we have been working on and edit the `Vagrantfile`. Remove the Puppet provisioning or comment it out and add:

    # Ansible test
    config.vm.provision "ansible" do |ansible|
      ansible.playbook = "ansible/site.yml"
      ansible.verbose = "vvvv"
      ansible.host_key_checking = "false"
      ansible.sudo_user = "root"
    end

And start the instance once again

    vagrant up --provision=cloudstack

Watch the output from the Ansible provisioning and once finished access the wordpress application that was just configured.
