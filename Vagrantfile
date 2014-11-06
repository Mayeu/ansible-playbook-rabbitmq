Vagrant.configure('2') do |config|
  config.vm.box = 'deb/jessie'
  config.vm.provision 'ansible' do |ansible|
    ansible.playbook = 'vagrant/site.yml'
    ansible.limit = 'all'
    ansible.sudo = true
    ansible.host_key_checking = false
  end

  config.vm.define 'rabbit-standalone' do |c|
    c.vm.host_name = 'rabbit-standalone'
  end
end
