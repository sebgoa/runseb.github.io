Apache libcloud
===============

Libcloud is a python module that abstracts the different APIs of most cloud providers. It offers a common API for the basic functionality of clouds list nodes,sizes,templates, create nodes etc...Libcloud can be used with CloudStack, OpenStack, Opennebula, GCE, AWS.

Check the CloudStack driver [documentation](https://libcloud.readthedocs.org/en/latest/compute/drivers/cloudstack.html)

Installation
------------

To install Libcloud refer to the libcloud [website](http://libcloud.apache.org). Or simply do:

    pip install apache-libcloud

Generic use of Libcloud with CloudStack
---------------------------------------

With libcloud installed, you can now open a Python interactive shell, create an instance of a CloudStack driver
and call the available methods via the libcloud API.

First you need to import the libcloud modules and create a CloudStack
driver.

    >>> from libcloud.compute.types import Provider
    >>> from libcloud.compute.providers import get_driver
    >>> Driver = get_driver(Provider.CLOUDSTACK)

Then, using your keys and endpoint, create a connection object. Note
that this is a local test and thus not secured. If you use a CloudStack
public cloud, make sure to use SSL properly (i.e `secure=True`). Replace the host and path with the ones of your public cloud. For exoscale use `host='http://api.exoscale.ch` and `path=/compute`

    >>> apikey='plgWJfZK4gyS3mlZLYq_u38zCm0bewzGUdP66mg'
    >>> secretkey='VDaACYb0LV9eNjeq1EhwJaw7FF3akA3KBQ'
    >>> host='http://localhost:8080'
    >>> path='/client/api'
    >>> conn=Driver(key=apikey,secret=secretkey,secure=False,host='localhost',port='8080',path=path)

With the connection object in hand, you now use the libcloud base api to
list such things as the templates (i.e images), the service offerings
(i.e sizes) and the zones (i.e locations)

    >>> conn.list_images()
    [<NodeImage: id=13ccff62-132b-4caf-b456-e8ef20cbff0e, name=tiny Linux, driver=CloudStack  ...>]
    >>> conn.list_sizes()
    [<NodeSize: id=ef2537ad-c70f-11e1-821b-0800277e749c, name=tinyOffering, ram=100 disk=0 bandwidth=0 price=0 driver=CloudStack ...>,
	<NodeSize: id=c66c2557-12a7-4b32-94f4-48837da3fa84, name=Small Instance, ram=512 disk=0 bandwidth=0 price=0 driver=CloudStack ...>,
	<NodeSize: id=3d8b82e5-d8e7-48d5-a554-cf853111bc50, name=Medium Instance, ram=1024 disk=0 bandwidth=0 price=0 driver=CloudStack ...>]
    >>> images=conn.list_images()
    >>> offerings=conn.list_sizes()

The `create_node` method will take an instance name, a template and an
instance type as arguments. It will return an instance of a
*CloudStackNode* that has additional extensions methods, such as
`ex_stop` and `ex_start`.

    >>> node=conn.create_node(name='toto',image=images[0],size=offerings[0])
    >>> help(node)
    >>> node.get_uuid()
    'b1aa381ba1de7f2d5048e248848993d5a900984f'
    >>> node.name
    u'toto'

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
    image=getimage('2c8bede9-c3b6-4450-9985-7b715d8e58c5')
    size=getsize('71004023-bb72-4a97-b1e9-bc66dfce9470')
    msd = MultiStepDeployment([script])

    # directly open the shell
    shell = InteractiveShellEmbed(banner1="Hello from Libcloud Shell !!")
    shell()

Set your API keys properly and execute the script. You can now explore the libcloud API interactively, try to start a node, and also deploy a node. For instance type `list` and press the tab key.

