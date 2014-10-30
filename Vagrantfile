Vagrant.configure('2') do |config|
  config.vm.box = 'jessie'
  config.vm.provision 'ansible' do |ansible|
    ansible.playbook = 'vagrant/site.yml'
    ansible.limit = 'all'
    ansible.sudo = true
    ansible.host_key_checking = false
    ansible.extra_vars = {
      rabbitmq_vhost_definitions: [
        { name: 'sensu' }
      ],
      rabbitmq_ssl: false,
      rabbitmq_conf_tcp_listeners_address: '0.0.0.0',
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
      ],
      rabbitmq_conf_env: {
        RABBITMQ_ROCKS: 'correct'
      }
    }
  end

  config.vm.define 'vagrantup' do |c|
    c.vm.host_name = 'vagrantup'
  end
end
