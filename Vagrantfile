# vim: set ft=ruby:
#
# list your boxes and ips here
boxes = [
  {
    :name => :rabbitbox1,
    :ip => "192.168.98.2"},
  {
    :name => :rabbitbox2,
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
    end
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
    #ansible.limit = 'all'
    ansible.sudo = true
    ansible.host_key_checking = false
    ansible.verbose = "vvv"
    ansible.groups = {
      "rabbitmq_servers" => ["rabbitbox1", "rabbitbox2"] }
    ansible.extra_vars = {
      rabbitmq_vhost_definitions: ['vhost1'],
      rabbitmq_ssl: false,
      rabbitmq_clustering: true,
      rabbitmq_federation: false,
      rabbitmq_cluster_nodes: ['rabbitbox1','rabbitbox2'],
      rabbitmq_cluster_cookie: 'KJSdK239sjdfkj32jacnwl7v32k',
      rabbitmq_plugins: [
        'rabbitmq_management_agent',
        'rabbitmq_management',
        'rabbitmq_shovel_management',
        'rabbitmq_shovel'],
      rabbitmq_users_definitions: [
        {
          vhost: 'vhost1',
          user: 'user1',
          password: 'placeholder'
        }
      ],
      rabbitmq_federation_configuration: [
        {
          name: 'test',
          vhost: '/',
          value: '{"uri":"amqp://server-name","expires":3600000}',
          local_username: 'user1'
        }
      ],
      rabbitmq_policy_configuration: [
        {
          name: 'policy',
          pattern: '.',
          vhost: 'user1',
          tags: 'federation-upstream-set=all'
        }
      ]
    }
  end
end
