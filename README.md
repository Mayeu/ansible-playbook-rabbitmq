# Rabbitmq Playbook

Playbook to install and configure rabbitmq. Will come with various
configuration tweaking later on.

If you wish to discuss modifications, or help to support more platforms, open
an issue.

## Installation

Use Ansible galaxy to install this playbook:

    $ ansible-galaxy install Mayeu.RabbitMQ,1.4.0

The `master` branch should currently be considered instable. Please avoid using
it for something else than test purpose :)

## Supported system

Currently only Debian Jessie and Wheezy on amd64 are supported. Patch welcome
to support other distribution or OS.

## Semantic versioning 2.0.0

Starting with the commit
[67c608826a140868a71854ce3129b5f3d67ddcce](https://github.com/Mayeu/ansible-playbook-rabbitmq/commit/67c608826a140868a71854ce3129b5f3d67ddcce),
this playbook use semantic versioning. Following the specification, and since
the playbook is used in production and I want to avoid breaking the
compatibility, the first version number is 1.0.0

The public API defined in the semantic versioning correspond to the settings
available to the user. Breaking the API (incrementing from `X.Y.Z` to
`(X+1).Y.Z`) in this context mean that the user need to change variable name
for its playbook to run.

Any new feature added (from `X.Y.Z` to `X.(Y+1).Z`) should have a working
default value that need no user interaction by default. If a feature addition
require user interaction, then it is not a minor upgrade, but a major one.

## Role Variables

### Installation

|Name|Type|Description|Default|
|----|----|-----------|-------|
`rabbitmq_os_package`|Bool|When true uses the default package proposed by the OS or distribution instead of the one distributed by RabbitMQ.|`false`|


### Environment

|Name|Type|Description|Default|
|----|----|-----------|-------|
`rabbitmq_conf_env`|Hash|Set environment variable|undef|

Exemple:

```yaml
rabbitmq_conf_env:
  RABBITMQ_ROCKS: correct
```

Will generate:

```
RABBITMQ_ROCKS="correct"
```

### Certificate

|Name|Type|Description|Default|
|----|----|-----------|-------|
`rabbitmq_cacert`|String|Path of the CA certificate file.|`files/rabbitmq_cacert.pem`
`rabbitmq_server_key`|String|Path of the SSL key file.|`files/rabbitmq_server_key.pem`
`rabbitmq_server_cert`|String|Path of the SSL certificate file.|`files/rabbitmq_server_cert.pem`
`rabbitmq_ssl`|Boolean|Define if we need to use SSL|`true`

### Default configuration file

|Name|Type|Description|Default|
|----|----|-----------|-------|
`rabbitmq_config_template_file`|String|Path to RabbitMQ configuration file template. Can be absolute or relative|`rabbitmq.config.j2`|
`rabbitmq_conf_tcp_listeners_address`|String|listening address for the tcp interface|`''`
`rabbitmq_conf_tcp_listeners_port`|Integer|listening port for the tcp interface|`5672`
`rabbitmq_conf_ssl_listeners_address`|String|listening address for the ssl interface|`'0.0.0.0'`
`rabbitmq_conf_ssl_listeners_port`|Integer|listening port for the ssl interface|`5671`
`rabbitmq_conf_ssl_options_cacertfile`|String|Path the CA certificate|`"/etc/rabbitmq/ssl/cacert.pem"`
`rabbitmq_conf_ssl_options_certfile`|String|Path to the server certificate|`"/etc/rabbitmq/ssl/server_cert.pem"`
`rabbitmq_conf_ssl_options_keyfile`|String|Path to the private key file|`"/etc/rabbitmq/ssl/server_key.pem"`
`rabbitmq_conf_ssl_options_fail_if_no_peer_cert`|Boolean|Value of the `fail_if_no_peer_cert` SSL option|`"true"`

### Plugins

|Name|Type|Description|Default|
|----|----|-----------|-------|
`rabbitmq_new_only`|String|Add plugins as new, without deactivating other plugins|`'no'`
`rabbitmq_plugins`|String|List|List of plugins to activate|`[]`

Tip: If you require additional configuration of plugins in the RabbitMQ configuration file, use `rabbit_config_template_file` to override the default template used. It is possible to add a custom `rabbitmq.config.j2` file in a `templates/` dir relative to your playbook for example.

### Vhost

|Name|Type|Description|Default|
|----|----|-----------|-------|
`rabbitmq_vhost_definitions`|List|Define the list of vhost to create|`[]`
`rabbitmq_users_definitions`|List of hash|Define the users, and associated vhost and password (see below)|`[]`

Defining the vhosts configuration

```yaml
rabbitmq_vhost_definitions:
  - name:    vhost1
    node:    node_name #Optionnal, default to "rabbit"
    tracing: yes       #Optionnal, default to "no"
```

Defining the users configuration:

```yaml
rabbitmq_users_definitions:
  - vhost:    vhost1
    user:     user1
    password: password1
    node:     node_name  # Optionnal, default to "rabbit"
  - vhost:    vhost1
    user:     user2
    password: password2
    force:    no
    tags:                # Optionnal, user tags
    - administrator
```

### Federation

|Name|Type|Description|Default|
|----|----|-----------|-------|
`rabbitmq_federation`|Boolean|Define if we need to setup federation|`false`
`rabbitmq_federation_configuration`|List of hashes|Define all the federation we need to setup|Not defined
`rabbitmq_policy_configuration`|List of hashes|Define all the federation we need to setup|Not defined

Defining the federation upstream configuration:

```yaml
rabbitmq_federation_upstream:
  - name: upstream name
    vhost: local vhost to federate
    value: json description of the federation
    local_username: the local username for the federation
```

See the [RabbitMQ documentation](http://www.rabbitmq.com/federation.html) for
the possible JSON value.

Defining the policy configuration:

```yaml
rabbitmq_policy_configuration:
  - name: name of the policy
    vhost: vhost where the policy will be applied
    pattern: pattern of the policy
    tags: description of the policy in dict form # exemple: "ha-mode=all"
```

## Files required

You have to put the needed certificates in your `files/` folder, for example:

    files/
     |- cacert.crt
     |- myserver_key.key
     |- myserver_cert.crt

And then configure the role:

```yaml
    rabbitmq_cacert: files/cacert.crt
    rabbitmq_server_key: files/myserver_key.key
    rabbitmq_server_cert: files/myserver_cert.crt
```

## Testing

There is some tests that try to provision a VM using Vagrant. Just launch them
with:

    $ vagrant up # for test with Debian jessie
    $ export VAGRANT_BOX_NAME='chef/centos-6.5' vagrant up # for test with Centos

You can change the VM used during test by setting the `VAGRANT_BOX_NAME` env
variable to something else than `deb/jessie`.

## License

BSD
