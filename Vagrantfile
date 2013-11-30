# -*- mode: ruby -*-
# vi: set ft=ruby :

# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.vm.box       = 'precise64'
  config.vm.box_url   = 'http://files.vagrantup.com/precise64.box'
  config.vm.host_name = 'rails-box.local'

  config.vm.network :forwarded_port, guest: 3000, host: 5000  # forward the default rails port
  config.vm.network :forwarded_port, guest: 3306, host: 4406  # forward the MySQL port
  config.vm.network :forwarded_port, guest: 5432, host: 6432  # forward the PostgreSQL port

  config.vm.synced_folder "app", "/home/vagrant/www/app", nfs: true

  # Use for hooking up Nginx/Passenger to Rails App and Capistrano for Deployment
  config.vm.network :private_network, ip: "192.168.30.11"

  # For some reason had to ssh and install new ruby then chef and re-provision
  # TD: So, might want to add those steps right here as well ...
  # config.vm.provision :shell, :inline => "gem install chef --version 11.6.0"

  config.vm.provision :chef_solo do |chef|
    chef.cookbooks_path = ["chef/cookbooks", "chef/site-cookbooks"]
    chef.roles_path     = [[:host, "chef/roles"]]
    chef.data_bags_path = [[:host, "chef/data_bags"]]

    chef.add_role "rails-dev"
    chef.json = {
        :mysql => {
          :server_root_password   => '',
          :server_debian_password => '',
          :server_repl_password   => ''
        },
        "postgresql" => {
          "password" => {
            "postgres" => ""
          }
        },
        "rvm" => {
          "rubies"  => ["2.0.0"],
          "global_gems" => [
              { 'name' => 'bundler' },
              { 'name' => 'rails'}
          ]
        }
      }
  end
end
