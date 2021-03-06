# -*- mode: ruby -*-
# vi: set ft=ruby :

BOXES = [
  {
    hostname: 'tc',
    ip: '192.168.80.10',
    ports: [8111, 5432],
    role: 'server',
    ram: 2048,
    cpus: 2,
    chef: {
      json: {
        'teamcity' => {
          'version' => '9.0.1',
          'password' => '$1$ByY03mDX$4pk9wp9bC19yB6pxSoVB81',
          'server' => {
            'backup' => 'file:///vagrant/datasets/TeamCity_Backup_9.0.1_Devdata.zip',
            'database' => {
              'username' => 'postgres',
              'password' => '3175bce1d3201d16594cebf9d7eb3f9d',
              'jar' => 'file:///usr/share/java/postgresql93-jdbc.jar',
              'connection_url' => 'jdbc\:postgresql\:///postgres'
            }
          }
        },
        'postgresql' => {
          'version' => '9.3',
          'enable_pgdg_yum' => true,
          'password' => {
            'postgres' => '3175bce1d3201d16594cebf9d7eb3f9d'
          },
          'contrib' => {
            'packages' => ['postgresql93-jdbc']
          }
        },
      },
      run_list: %w(recipe[postgresql::contrib] recipe[chef-teamcity::server])
    }
  },
  {
    hostname: 'a1',
    ip: '192.168.80.11',
    ports: [],
    role: 'agent',
    ram: 2048,
    cpus: 2,
    chef: {
      json: {
        'teamcity' => {
          'password' => '$1$ByY03mDX$4pk9wp9bC19yB6pxSoVB81',
          'agent' => {
            'name' => 'agent01',
            'server_uri' => 'http://192.168.80.10:8111'
          }
        },
      },
      run_list: %w(recipe[chef-teamcity::agent])
    }
  }
]

Vagrant.configure(2) do |config|

  if Vagrant.has_plugin?("vagrant-cachier")
    config.cache.scope = :box
  end

  BOXES.each do |node|
    config.vm.define node[:hostname] do |node_config|
      node_config.vm.hostname = node[:hostname]
      node_config.vm.box = "chef/centos-6.5"

      node_config.vm.network :private_network, ip: node[:ip]

      node[:ports].each do |port|
        node_config.vm.network :forwarded_port, guest: port, host: port
      end

      node_config.vm.provider :virtualbox do |box|
        box.customize ['modifyvm', :id, '--name', node[:hostname]]
        box.customize ['modifyvm', :id, '--memory', node[:ram]]
        box.customize ['modifyvm', :id, '--cpus', node[:cpus]]
      end

      node_config.vm.provision :chef_solo do |chef|
        chef.custom_config_path = 'solo.rb'
        chef.json = node[:chef][:json]
        chef.run_list = node[:chef][:run_list]
      end
    end
  end
end
