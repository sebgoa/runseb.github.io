CloudMonkey, an interactive shell to your cloud
===============================================

The exoscale cloud is based on CloudStack. It exposes the CloudStack native API. Let's use CloudMonkey, the ACS cli.
 
    pip install cloudmonkey
    cloudmonkey
 
The full documentation for cloudmonkey is on the [wiki](https://cwiki.apache.org/confluence/display/CLOUDSTACK/CloudStack+cloudmonkey+CLI)
 
    set port 443
    set protocol https
    set path /compute
    set host api.exoscale.ch
    set apikey <yourapikey>
    set secretkey <secretkey>
 
Explore the ACS native API with CloudMonkey and tab tab....

Try to create a `sshkeypair` with `create sshkeypair`, a `securitygroup` with `create securitygroup` and add some rules to it.


Tabular Output
--------------------

The number of key/value pairs returned by the api calls can be large
resulting in a very long output. To enable easier viewing of the output,
a tabular formatting can be setup. You may enable tabular listing and
even choose set of column fields, this allows you to create your own
field using the filter param which takes in comma separated argument. If
argument has a space, put them under double quotes. The create table
will have the same sequence of field filters provided

To enable it, use the *set* function and create filters like so:

    > set display table
    > list zones filter=name,id
    count = 1
    zone:
    +--------+--------------------------------------+
    |  name  |                  id                  |
    +--------+--------------------------------------+
    | CH-GV2 | 1128bd56-b4d9-4ac6-a7b9-c715b187ce11 |
    +--------+--------------------------------------+
        
Starting a Virtual Machine instance with CloudMonkey
------------------------------------------------------------------------

To start a virtual machine instance we will use the *deployvirtualmachine* call.

    cloudmonkey>deploy virtualmachine -h
    Creates and automatically starts a virtual machine based on a service offering, disk offering, and template.
    Required args: serviceofferingid templateid zoneid
    Args: account diskofferingid displayname domainid group hostid hypervisor ipaddress iptonetworklist isAsync keyboard keypair name networkids projectid securitygroupids securitygroupnames serviceofferingid size startvm templateid userdata zoneid

The required arguments are *serviceofferingid, templateid and zoneid*

In order to specify the template that we want to use, we can list all
available templates with the following call:

     > list templates filter=id,displaytext templatefilter=executable
    count = 36
    template:
    +--------------------------------------+------------------------------------------+
    |                  id                  |               displaytext                |
    +--------------------------------------+------------------------------------------+
    | 3235e860-2f00-416a-9fac-79a03679ffd8 | Windows Server 2012 R2 WINRM 100GB Disk  |
    | 20d4ebc3-8898-431c-939e-adbcf203acec |   Linux Ubuntu 13.10 64-bit 10 GB Disk   |
    | 70d31a38-c030-490b-bca9-b9383895ade7 |   Linux Ubuntu 13.10 64-bit 50 GB Disk   |
    | 4822b64b-418f-4d6b-b64e-1517bb862511 |  Linux Ubuntu 13.10 64-bit 100 GB Disk   |
    | 39bc3611-5aea-4c83-a29a-7455298241a7 |  Linux Ubuntu 13.10 64-bit 200 GB Disk   |
    ...<snipped>

Similarly to get the *serviceofferingid* you would do:

    > list serviceofferings filter=id,name
    count = 7
    serviceoffering:
    +--------------------------------------+-------------+
    |                  id                  |     name    |
    +--------------------------------------+-------------+
    | 71004023-bb72-4a97-b1e9-bc66dfce9470 |    Micro    |
    | b6cd1ff5-3a2f-4e9d-a4d1-8988c1191fe8 |     Tiny    |
    | 21624abb-764e-4def-81d7-9fc54b5957fb |    Small    |
    | b6e9d1e8-89fc-4db3-aaa4-9b4c5b1d0844 |    Medium   |
    | c6f99499-7f59-4138-9427-a09db13af2bc |    Large    |
    | 350dc5ea-fe6d-42ba-b6c0-efb8b75617ad | Extra-large |
    | a216b0d1-370f-4e21-a0eb-3dfc6302b564 |     Huge    |
    +--------------------------------------+-------------+

Note that we can use the linux pipe as well as standard linux commands
within the interactive shell. Finally we would start an instance with
the following call:

    cloudmonkey>deploy virtualmachine templateid=20d4ebc3-8898-431c-939e-adbcf203acec zoneid=1128bd56-b4d9-4ac6-a7b9-c715b187ce11 serviceofferingid=71004023-bb72-4a97-b1e9-bc66dfce9470
    id = 5566c27c-e31c-438e-9d97-c5d5904453dc
    jobid = 334fbc33-c720-46ba-a710-182af31e76df

This is an asynchronous job, therefore it returns a `jobid`, you can query the state of this job with:

    > query asyncjobresult jobid=334fbc33-c720-46ba-a710-182af31e76df
    accountid = b8c0baab-18a1-44c0-ab67-e24049212925
    cmd = com.cloud.api.commands.DeployVMCmd
    created = 2014-03-05T13:40:18+0100
    jobid = 334fbc33-c720-46ba-a710-182af31e76df
    jobinstanceid = 5566c27c-e31c-438e-9d97-c5d5904453dc
    jobinstancetype = VirtualMachine
    jobprocstatus = 0
    jobresultcode = 0
    jobstatus = 0
    userid = 968f6b4e-b382-4802-afea-dd731d4cf9b9

Once the machine is started you can list it:

    > list virtualmachines filter=id,displayname
    count = 1
    virtualmachine:
    +--------------------------------------+--------------------------------------+
    |                  id                  |             displayname              |
    +--------------------------------------+--------------------------------------+
    | 5566c27c-e31c-438e-9d97-c5d5904453dc | 5566c27c-e31c-438e-9d97-c5d5904453dc |
    +--------------------------------------+--------------------------------------+

The instance would be stopped with:

    > stop virtualmachine id=5566c27c-e31c-438e-9d97-c5d5904453dc
    jobid = 391b4666-293c-442b-8a16-aeb64eef0246

    > list virtualmachines filter=id,displayname,state
    count = 1
    virtualmachine:
    +--------------------------------------+--------------------------------------+---------+
    |                  id                  |             displayname              |  state  |
    +--------------------------------------+--------------------------------------+---------+
    | 5566c27c-e31c-438e-9d97-c5d5904453dc | 5566c27c-e31c-438e-9d97-c5d5904453dc | Stopped |
    +--------------------------------------+--------------------------------------+---------+
        
The *ids* that you will use will differ from this example. Make sure you use the ones that corresponds to your CloudStack cloud.

With CloudMonkey all CloudStack APIs are available.

