Remote Execution
----------------

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

