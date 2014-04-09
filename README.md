storm-vagrant
=============

Vagrant config to create a virtualized Storm cluster

How to use it?
==============

First, run pre_vagrant.sh script, that installs necessary vagrant plugins (assuming that you don't have it yet).
Then, just run vagrant up. During the provisioning process, vagrant will ask you for sudo password, because it wants 
to add create vagrant nodes to the /etc/hosts.

And then, you can play with the storm. For now, what I was doing is:
* log in to nimbus node.
    vagrant ssh nimbus
OR
    ssh vagrant@nimbus # password "vagrant"
* launch some topology on storm.
    storm jar path/to/allmycode.jar org.me.MyTopology arg1 arg2 arg3
  look here for more pro info: https://github.com/nathanmarz/storm/wiki/Running-topologies-on-a-production-cluster
