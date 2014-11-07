BOX_NAME = ENV['VAGRANT_BOX_NAME'] || 'deb/jessie'

Vagrant.configure('2') do |config|
  config.vm.box = BOX_NAME
  config.vm.provision 'ansible' do |ansible|
    ansible.playbook = 'vagrant/site.yml'
    ansible.limit = 'all'
    ansible.sudo = true
    ansible.host_key_checking = false
    ansible.groups = {
      'rabbit-cluster' => [
        'rabbit-cluster1',
        'rabbit-cluster2',
        'rabbit-cluster3'
      ]
    }
  end

  config.vm.provider 'virtualbox' do |v|
    v.customize ['modifyvm', :id, '--memory', 512]
  end

  config.vm.define 'rabbit-standalone' do |c|
    c.vm.host_name = 'rabbit-standalone'
  end

  (1..3).each do |i|
    vm_name = "rabbit-cluster#{i}"
    config.vm.define vm_name do |c|
      c.vm.host_name = vm_name
      c.vm.network 'private_network', ip: "192.168.100.#{11 + i}" # eth1
    end
  end
end
