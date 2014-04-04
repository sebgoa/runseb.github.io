Ansible
=======

Our last exercise for this tutorial will be an introduction to [Ansible](http://ansibleworks.com). Ansible is a new configuration management systems based on ssh communications with the instances and a no-server setup. It is easy to install and get [started](http://docs.ansible.com/intro.html). Of course it can be used in conjunction with Vagrant.

Install and remote execution
----------------------------

First install *ansible*:
    
    pip install ansible

Or get it via packages `yum install ansible`, `apt-get install ansible` if you have set the proper repositories.

If you kept the instances from the previous exercise running, create an *inventory* `inv` file with the IP addresses. Like so

   185.1.2.3
   185.3.4.5

Then run your first ansible command: `ping`:

    ansible all -i inv -m ping

You should see the following output:

    185.1.2.3 | success >> {
        "changed": false, 
        "ping": "pong"
    }

    185.3.4.5 | success >> {
        "changed": false, 
        "ping": "pong"
    }

And see how you can use Ansible as a remote execution framework:

    $ ansible all -i inv -a "/bin/echo hello"
    185.1.2.3 | success | rc=0 >>
    hello

    185.3.4.5 | success | rc=0 >>
    hello

Now check all the great Ansible [examples](https://github.com/ansible/ansible-examples), pick one, download it via github and try to configure your instances with `ansible-playbook`

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


