# -*- mode: ruby -*-
# vi: set ft=ruby :

require 'yaml'

cluster = YAML::load(open('cluster.yaml'))
cluster['nodes'].each { |k,v| cluster['nodes'][k] =
    cluster['defaults'].merge(v)}
cluster = cluster['nodes']

ANSIBLE_GROUPS = Hash.new
cluster.each do |n, ps|
  ps['ansible_groups'].each do |group|
    unless ANSIBLE_GROUPS.key?(group)
      ANSIBLE_GROUPS[group] = Array.new
    end
    ANSIBLE_GROUPS[group] << n
  end
end

VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|

  config.ssh.forward_agent = true

  cluster.each_with_index do |(n, props),index|
    config.vm.define n do |v|
      v.vm.hostname = n + props['tld']
      v.vm.network 'private_network', ip: props['ip'],
          auto_configure: props['net_auto']
      v.vm.box = 'chef/ubuntu-12.04'
      v.vm.provider "virtualbox" do |vbox|
        #vbox.gui = true
        vbox.memory = props['memory']
        vbox.customize ['modifyvm', :id, '--natdnshostresolver1', 'on']
        vbox.customize ['modifyvm', :id, '--natdnsproxy1', 'on']
      end
      if index == cluster.length - 1
        v.vm.provision 'ansible' do |ansible|
          ansible.groups = ANSIBLE_GROUPS
          ansible.playbook = 'provisioning/playbook.yaml'
          ansible.sudo = true
          ansible.limit = 'all'
        end
      end
    end
  end



end
