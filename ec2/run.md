Run ec2stack
------------

Once you configured it, run the `ec2stack` server:

    $ec2stack

By default it will run on localhost and port 5000

Register a user
---------------

To be on the safe side, we are going to upgrade the `requests` module

    $pip install --upgrade requests

Now you need to register your API keys from exoscale

    $ec2stack-register http://localhost:5000 <your API accesskey> <your API secret key>

The command should return a `Successfully Registered!` message.

In addition you need to register your exoscale API keys with the `aws` CLI:

    $aws configure
    AWS Access Key ID [None]: PQogHs2sk_3uslfvrASjQFDlZbt0mEDd14iNSgCsgB0DoCjAFakosJINePC4JsFiyHzQ3LwlWIXZbVdNPUn-mg
    AWS Secret Access Key [None]: aHuDB2ewpgxVuQlvD9P1o313BioI1W4vFCxQpTCqGvbqj3Y6mVZo-paBbYyE3W_TQKEirnTKENNRC5NR5cUjEg
    Default region name [None]: CH-GV2
    Default output format [None]:

You can see these settings in the `~/.aws/config` mentioned earlier.
Check the AWS CLI [reference](http://docs.aws.amazon.com/cli/latest/reference/) for further customization.
The output format can be `json`, `text` or `table`.

You are now ready to try the AWS interface
