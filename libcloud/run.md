Creating and Deploying a node
-----------------------------

As with previous clients try to start an instance

    conn.create_node(name='foobar',image=image,size=size)

This simple command will us the default security group and your default keypair.

Explore the `create_node` method to see how to specify a security group and a SSH key pair that is not the default one.

Libcloud also allows to start an instance and execute a script to configure it.

Looking through the interactive shell script you saved in the previous section, you should see the `ScriptDeployment()` and `MultiStepDeployment()` classes.

Edit the file so the script specified in `ScriptDeployment` is the same that you used earlier to deploy wordpress from the exoscale UI, for example something like this:

    wordpress=open('wordpress.sh','r')
    script=ScriptDeployment(wordpress)

Now relaunch the interactive shell and deploy the node with:

    conn.deploy_node(name='wordpresslibcloud',image=image,size=size,ex_keyname='exoscale',pub_key_identity='<path/to/private/key',deploy=msd)

When it completes, check the `stdout` with:

    script.stdout

Open your browser and go to `http://<ip of instance started>` if all went well, you should see wordpress :)
