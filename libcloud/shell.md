libcloud with exoscale
----------------------

Libcloud also has an exoscale specific driver. For complete description see this recent [post](https://www.exoscale.ch/syslog/2014/01/27/licloud-guest-post/) from Tomaz Murauz the VP of Apache Libcloud.

To get you started quickly, save the following script in a .py file.

    #!/usr/bin/env python

    import sys
    import os

    from IPython.terminal.embed import InteractiveShellEmbed
    from libcloud.compute.types import Provider
    from libcloud.compute.providers import get_driver
    from libcloud.compute.deployment import ScriptDeployment
    from libcloud.compute.deployment import MultiStepDeployment

    apikey=os.getenv('EXOSCALE_API_KEY')
    secretkey=os.getenv('EXOSCALE_SECRET_KEY')
    Driver = get_driver(Provider.EXOSCALE)
    conn=Driver(key=apikey,secret=secretkey)

    print conn.list_locations()

    def listimages():
        for i in conn.list_images():
            print i.id, i.extra['displaytext']

    def listsizes():
        for i in conn.list_sizes():
            print i.id, i.name

    def getimage(id):
        return [i for i in conn.list_images() if i.id == id][0]

    def getsize(id):
        return [i for i in conn.list_sizes() if i.id == id][0]

    script=ScriptDeployment("/bin/date")
    image=getimage('8c7e60ae-3a30-4031-a3e6-29832d85d7cb')
    size=getsize('71004023-bb72-4a97-b1e9-bc66dfce9470')
    msd = MultiStepDeployment([script])

    # directly open the shell
    shell = InteractiveShellEmbed(banner1="Hello from Libcloud Shell !!")
    shell()

Set your API keys properly.

Start this shell by executing the script:

    $ ./libshell.py
    [<NodeLocation: id=1128bd56-b4d9-4ac6-a7b9-c715b187ce11, name=CH-GV2, country=Unknown, driver=Exoscale>]
    Hello from Libcloud Shell !!
    In [1]: 

At the prompt type `conn.list` and press the tab key to see the list of available `list` apis.


You can now explore the libcloud API interactively.

Try to create a SSH key pair with `conn.create_key_pair()`, list the key pairs with `conn.list_key_pairs()`, delete it...

