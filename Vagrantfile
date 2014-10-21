# vim: set ft=ruby:
#
# list your boxes and ips here
boxes = [
  {
    :name => :zabbixbox1,
    :ip => "192.168.98.2"},
  {
    :name => :zabbixbox2,
    :ip => "192.168.98.3"}]


Vagrant.configure('2') do |config|

  if Vagrant.has_plugin?("vagrant-hostmanager")
    config.hostmanager.enabled      = true
    config.hostmanager.include_offline  = true
    config.hostmanager.manager_host   = true
    config.vm.provision :hostmanager
  end

  boxes.each do |opts|
    config.vm.define opts[:name] do |machine|
      # default to use a debian vagrant box,
      # unless we define the VAGRANT_OS variable telling it to do otherwise
      if ENV['VAGRANT_OS'] == "Centos65"
        machine.vm.box = 'centos65'
        machine.vm.box_url = 'https://github.com/2creatives/vagrant-centos/releases/download/v6.5.3/centos65-x86_64-20140116.box'
      else
        machine.vm.box = 'jessie'
        machine.vm.box_url = 'https://downloads.sourceforge.net/project/vagrantboxjessie/debian80.box'
      end

      machine.vm.hostname = opts[:name]
      machine.vm.network :private_network, ip: "#{opts[:ip]}"

      machine.vm.provider :virtualbox do |virtual|
        virtual.customize ['modifyvm', :id, '--name', opts[:name].to_s ]
      end

      config.vm.provision 'ansible' do |ansible|

        # these are really handy for debugging,
        # simply export TAGS='tagname'
        # or export START_AT_TASK='start from here'
        if defined? ENV['TAGS']
          ansible.tags = ENV['TAGS']
        end
        if defined? ENV['START_AT_TASK']
          ansible.start_at_task = ENV['START_AT_TASK']
        end

        ansible.playbook = 'vagrant/site.yml'
        ansible.limit = 'all'
        ansible.sudo = true
        ansible.host_key_checking = false
        ansible.verbose = "vvv"
        ansible.extra_vars = {
          rabbitmq_vhost_definitions: ['sensu'],
          rabbitmq_ssl: false,
          rabbitmq_clustering: false,
          rabbitmq_federation: false,
          rabbitmq_users_definitions: [
            {
              vhost: 'sensu',
              user: 'sensu',
              password: 'placeholder'
            }
          ],
          rabbitmq_federation_configuration: [
            {
              name: 'test',
              vhost: '/',
              value: '{"uri":"amqp://server-name","expires":3600000}',
              local_username: 'sensu'
            }
          ],
          rabbitmq_policy_configuration: [
            {
              name: 'policy',
              pattern: '.',
              vhost: 'sensu',
              tags: 'federation-upstream-set=all'
            }
          ]
        }
      end
    end
  end
end
