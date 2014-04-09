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

Try a few `list` calls and play with the filter
