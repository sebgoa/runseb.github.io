Generic use of Libcloud with CloudStack
---------------------------------------

With libcloud installed, you can now open a Python interactive shell, create an instance of a CloudStack driver
and call the available methods via the libcloud API.

This is a basic introduction, the exercise follows in the next section.

First you need to import the libcloud modules and create a CloudStack
driver.

    >>> from libcloud.compute.types import Provider
    >>> from libcloud.compute.providers import get_driver
    >>> Driver = get_driver(Provider.CLOUDSTACK)

Then, using your keys and endpoint, create a connection object. Note
that this is a local test and thus not secured. If you use a CloudStack
public cloud, make sure to use SSL properly (i.e `secure=True`). Replace the host and path with the ones of your public cloud. For exoscale use `host='http://api.exoscale.ch'` and `path=/compute`

    >>> apikey='plgWJfZK4gyS3mlZLYq_u38zCm0bewzGUdP66mg'
    >>> secretkey='VDaACYb0LV9eNjeq1EhwJaw7FF3akA3KBQ'
    >>> host='http://api.exoscale.ch'
    >>> port='443'
    >>> secure=True
    >>> path='/compute'
    >>> conn=Driver(key=apikey,secret=secretkey,secure=secure,host=host,port=port,path=path)

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
