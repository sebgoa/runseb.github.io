Playing with multi-machines configuration
-----------------------------------------

Vagrant is also very interesting because you can start multiple machines at [once](http://docs.vagrantup.com/v2/multi-machine/index.html). Edit the `Vagrantfile` to add a `web` and a `db` machine. Add the cloudstack specific information and specify different bootstrap scripts.

    config.vm.define "web" do |web|
      web.vm.box = "tutorial"
    end

    config.vm.define "db" do |db|
      db.vm.box = "tutorial"
    end

You can control each machine separately `vagrant up web`, `vagrant up db` or all at once in parallel `vagrant up`

Let the fun begin. Pick your favorite configuration management tool, decide what you want to provision, setup your recipes and launch the instances.

