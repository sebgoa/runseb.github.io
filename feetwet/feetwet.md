Discover the UI and start an instance 
-------------------------------------
 
Browse the UI, identify the `security groups` and `keypairs` sections.

![Sec Group and Key Pair](../images/secgkeys.png)

Create a rule in your default security group to allow inbound traffic on port 22 (ssh)

![Security Group ssh rule](../images/secgssh.png)

Create a keypair and store the private key on your machine. CloudStack does not store the private key, so make sure you save it in your `~/.ssh` directory.

![Key Pairs](./images/keypairs.png)

Start an instance by clicking on the `Instances` tab and then the `Add` button.

You can then give a name to your instance, select a template, a machine type, a security group and a keypair.

![Add Instance](./images/addinstance.png)

ssh to the instance using the keys that you created earlier:

   ssh -i /path/to/key root@<IP of instance>


