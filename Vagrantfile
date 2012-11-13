# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant::Config.run do |config|

  nodes = [{:vm_name => :riak1, :network_ip => "33.33.33.11",:http_port => 8091, :pbc_port => 8081},
           {:vm_name => :riak2, :network_ip => "33.33.33.12",:http_port => 8092, :pbc_port => 8082},
           {:vm_name => :riak3, :network_ip => "33.33.33.13",:http_port => 8093, :pbc_port => 8083},
           {:vm_name => :riak4, :network_ip => "33.33.33.14",:http_port => 8094, :pbc_port => 8084}]


  nodes.each do |node|

    config.vm.define node[:vm_name] do |riak_config|

      riak_config.vm.box = "lucid64"
      riak_config.vm.box_url = "http://files.vagrantup.com/lucid64.box"

      riak_config.vm.network :hostonly, node[:network_ip]

      riak_config.vm.forward_port(8098, node[:http_port])
      riak_config.vm.forward_port(8087, node[:pbc_port])

      riak_config.vm.customize do |vm|
        vm.memory_size = 512
      end

      riak_config.vm.provision :chef_solo do |chef|

        chef.cookbooks_path = ['cookbooks']

        chef.add_recipe("riak")

        chef.json.merge!(:riak => {:args => {:"-name" => "#{node[:vm_name]}@#{node[:network_ip]}"}})
      end

    end

  end
end
