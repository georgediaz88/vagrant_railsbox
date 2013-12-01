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

  config.vm.synced_folder "homegrown_apps", "/home/vagrant/www/apps/homegrown_apps", nfs: true

  # Use for hooking up Nginx/Passenger to Rails App and Capistrano for Deployment
  config.vm.network :private_network, ip: "192.168.30.11"

  # For some reason had to ssh and install new ruby then chef and re-provision
  # config.vm.provision :shell, :inline => "gem install chef --version 11.6.0"
  # TD: ruby-2.0.0-p-247 to work with chef latest bleeding version

  config.vm.provision :shell, :path => "install.sh"

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
        :nginx => {
          :version => "1.4.1",
          :dir => "/etc/nginx",
          :log_dir => "/var/log/nginx",
          :binary => "/opt/nginx-1.4.1/sbin",
          :user => "root",
          :init_style => "init",
          :source => {
            :use_existing_user => true,
            :modules => [
              "nginx::http_stub_status_module",
              "nginx::http_ssl_module",
              "nginx::http_gzip_static_module",
              "nginx::passenger"
            ]
          },
          :passenger => {
            :version => "3.0.21",
            :ruby => "/usr/local/rvm/rubies/ruby-1.9.3-p327/bin/ruby",
            :root => "/usr/local/rvm/gems/ruby-1.9.3-p327/gems/passenger-3.0.21"
          }
        },
        "rvm" => {
          "rubies"  => ["1.9.3-p327"],
          "global_gems" => [
              { 'name' => 'bundler' },
              { 'name' => 'rails'}
          ]
        }
      }
  end
end
