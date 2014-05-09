
Vagrant.configure("2") do |config|
  config.vm.box = "jessie"
  config.vm.hostname = "vagrantup"

  config.vm.provision "ansible" do |ansible|
    ansible.playbook = "test.yml"
    ansible.host_key_checking = false
  end
end
