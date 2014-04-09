Configuration
-------------

At the `cloudmonkey` prompt use the `set` command to point to `exoscale`:

    >set port 443
    >set protocol https
    >set path /compute
    >set host api.exoscale.ch
    >set apikey <yourapikey>
    >set secretkey <secretkey>

These settings are saved in `~/.cloudmonkey/config`, check that file and see the settings.
You can edit that file directly.

You are now ready to make calls to `exoscale` using CloudMonkey.
