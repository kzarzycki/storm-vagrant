# -*- mode: ruby -*-
# vi: set ft=ruby :

VAGRANTFILE_API_VERSION = "2"
STORM_VERSION = "apache-storm-0.9.1-incubating"
STORM_ARCHIVE = "#{STORM_VERSION}.zip"

network = '192.168.2.'

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  
  config.hostmanager.manage_host = true
  config.hostmanager.enabled = true

  config.vm.box = "precise"
  config.vm.box_url = "http://files.vagrantup.com/precise64.box"


  if(!File.exist?(STORM_ARCHIVE))
    `wget -N http://ftp.ps.pl/pub/apache/incubator/storm/#{STORM_VERSION}/#{STORM_VERSION}.zip`
  end
  
  config.vm.define "zookeeper" do |zookeeper|
    ip = network + 3.to_s
    zookeeper.vm.network "private_network", ip: ip 
    zookeeper.vm.hostname = "zookeeper"
    zookeeper.vm.provision "shell", path: "install-zookeeper.sh"
  end

  config.vm.define "nimbus" do |nimbus|
    ip = network + 4.to_s
    nimbus.vm.network "private_network", ip: ip
    nimbus.vm.hostname = "nimbus"
    
    nimbus.vm.provision "shell", path: "install-storm.sh", args: STORM_VERSION
    nimbus.vm.provision "shell", path: "config-supervisord.sh", args: "nimbus"
    nimbus.vm.provision "shell", path: "config-supervisord.sh", args: "ui"
    nimbus.vm.provision "shell", path: "config-supervisord.sh", args: "drpc"
    nimbus.vm.provision "shell", path: "start-supervisord.sh"
  end

  config.vm.define "supervisor1" do |supervisor|
    ip = network + 5.to_s
    supervisor.vm.network "private_network", ip: ip
    supervisor.vm.hostname = "supervisor1"
    
    supervisor.vm.provision "shell", path: "install-storm.sh", args: STORM_VERSION
    supervisor.vm.provision "shell", path: "config-supervisord.sh", args: "supervisor"
    supervisor.vm.provision "shell", path: "config-supervisord.sh", args: "logviewer"
    supervisor.vm.provision "shell", path: "start-supervisord.sh"
    
  end
  
  config.vm.define "supervisor2" do |supervisor|
    ip = network + 6.to_s
    supervisor.vm.network "private_network", ip: ip 
    supervisor.vm.hostname = "supervisor2"
    
    supervisor.vm.provision "shell", path: "install-storm.sh", args: STORM_VERSION
    supervisor.vm.provision "shell", path: "config-supervisord.sh", args: "supervisor"
    supervisor.vm.provision "shell", path: "config-supervisord.sh", args: "logviewer"
    supervisor.vm.provision "shell", path: "start-supervisord.sh"
    
  end
  
  # Provider-specific configuration so you can fine-tune various
  # backing providers for Vagrant. These expose provider-specific options.
  #
  config.vm.provider :virtualbox do |vb|
    #   # Don't boot with headless mode
    vb.gui = false
    # (kza) some bug in vagrant/ubuntu prevents network from working withou the following commands.
    vb.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
    vb.customize ["modifyvm", :id, "--natdnsproxy1", "on"]
    #
    #   # Use VBoxManage to customize the VM. For example to change memory:
    #   vb.customize ["modifyvm", :id, "--memory", "1024"]
  end

  
  # Some other options left for documentation purposes ... 
  #
  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine. In the example below,
  # accessing "localhost:8080" will access port 80 on the guest machine.
  # config.vm.network :forwarded_port, guest: 80, host: 8080

  # Create a public network, which generally matched to bridged network.
  # Bridged networks make the machine appear as another physical device on
  # your network.
  # config.vm.network :public_network

  # If true, then any SSH connections made will enable agent forwarding.
  # Default value: false
  # config.ssh.forward_agent = true

  # Share an additional folder to the guest VM. The first argument is
  # the path on the host to the actual folder. The second argument is
  # the path on the guest to mount the folder. And the optional third
  # argument is a set of non-required options.
  # config.vm.synced_folder "../data", "/vagrant_data"

  # Enable provisioning with chef solo, specifying a cookbooks path, roles
  # path, and data_bags path (all relative to this Vagrantfile), and adding
  # some recipes and/or roles.
  #
  # config.vm.provision :chef_solo do |chef|
  #   chef.cookbooks_path = "../my-recipes/cookbooks"
  #   chef.roles_path = "../my-recipes/roles"
  #   chef.data_bags_path = "../my-recipes/data_bags"
  #   chef.add_recipe "mysql"
  #   chef.add_role "web"
  #
  #   # You may also specify custom JSON attributes:
  #   chef.json = { :mysql_password => "foo" }
  # end

  # Enable provisioning with chef server, specifying the chef server URL,
  # and the path to the validation key (relative to this Vagrantfile).
  #
  # The Opscode Platform uses HTTPS. Substitute your organization for
  # ORGNAME in the URL and validation key.
  #
  # If you have your own Chef Server, use the appropriate URL, which may be
  # HTTP instead of HTTPS depending on your configuration. Also change the
  # validation key to validation.pem.
  #
  # config.vm.provision :chef_client do |chef|
  #   chef.chef_server_url = "https://api.opscode.com/organizations/ORGNAME"
  #   chef.validation_key_path = "ORGNAME-validator.pem"
  # end
  #
  # If you're using the Opscode platform, your validator client is
  # ORGNAME-validator, replacing ORGNAME with your organization name.
  #
  # If you have your own Chef Server, the default validation client name is
  # chef-validator, unless you changed the configuration.
  #
  #   chef.validation_client_name = "ORGNAME-validator"
end
