configure ec2stack
------------------

`ec2stack` can be configured with `ec2stack-configure` command. To set it up with [exoscale](http://exoscale.ch) do:

    $ec2stack-configure
    EC2Stack bind address [0.0.0.0]: 
    EC2Stack bind port [5000]: 
    Cloudstack host [localhost]: api.exoscale.ch
    Cloudstack port [8080]: 443
    Cloudstack protocol [http]: https
    Cloudstack path [/client/api]: /compute
    Cloudstack custom disk offering name [Custom]: 
    Cloudstack default zone name: CH-GV2
    Do you wish to input instance type mappings? (Yes/No): Yes
    Insert the AWS EC2 instance type you wish to map: m1.small
    Insert the name of the instance type you wish to map this to: Tiny
    Do you wish to add more mappings? (Yes/No): No
    INFO  [alembic.migration] Context impl SQLiteImpl.
    INFO  [alembic.migration] Will assume non-transactional DDL.

Note that we created a mapping between the AWS m1.small instance type to the `Tiny` instance type in exoscale.
You could add more mappings.
