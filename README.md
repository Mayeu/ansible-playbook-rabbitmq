# Rabbitmq Playbook

Playbook to install and configure rabbitmq. Will come with various
configuration tweaking later on.

If you wish to discuss modifications, or help to support more plateforms, open
an issue.

## Supported system

Currently only Debian Jessie and Wheezy on amd64 is supported. Patch welcome to
support other OS.

### Role Variables

#### Certificate
|Name|Type|Description|Default|
|----|----|-----------|-------|
`rabbitmq_cacert`|String|Name of the CA certificate file. Will be prefixed by `rabbitmq_` and postfixed by `.pem`|`cacert`
`rabbitmq_server_key`|String|Name of the SSL key file. Will be prefixed by `rabbitmq_` and postfixed by `.pem`|`server_key`
`rabbitmq_cert_key`|String|Name of the SSL certificate file. Will be prefixed by `rabbitmq_` and postfixed by `.pem`|`server_cert`

#### Default configuration file

|Name|Type|Description|Default|
|----|----|-----------|-------|
`rabbitmq_conf_tcp_listeners_address`|String|listening address for the tcp interface|`''`
`rabbitmq_conf_tcp_listeners_port`|Integer|listening port for the tcp interface|`5672`
`rabbitmq_conf_ssl_listeners_address`|String|listening address for the ssl interface|'0.0.0.0'`
`rabbitmq_conf_ssl_listeners_port`|Integer|listening port for the ssl interface|`5671`
`rabbitmq_conf_ssl_options_cacertfile`|String|Path the CA certificate|`"/etc/rabbitmq/ssl/cacert.pem"`
`rabbitmq_conf_ssl_options_certfile`|String|Path to the server certificate|`"/etc/rabbitmq/ssl/server_cert.pem"`
`rabbitmq_conf_ssl_options_keyfile`|String|Path to the private key file|`"/etc/rabbitmq/ssl/server_key.pem"`
`rabbitmq_conf_ssl_options_fail_if_no_peer_cert`|Boolean|Value of the `fail_if_no_peer_cert` SSL option|"true"

#### Vhost

|Name|Type|Description|Default|
|----|----|-----------|-------|
`rabbitmq_vhost_definitions`|List|Define the list of vhost to create|Not defined
`rabbitmq_users_definitions`|List of hash|Define the users, and associated vhost and password (see below)|Not defined

Defining the users configuration:

```yaml
rabbitmq_users_definitions:
  - vhost   : vhost1
    user    : user1
    password: password1
  - vhost   : vhost1
    user    : user2
    password: password2
```
#### Federation

|Name|Type|Description|Default|
|----|----|-----------|-------|
`rabbitmq_federation`|Boolean|Define if we need to setup federation|`false`
`rabbitmq_federation_upstream|List of hashes|Define all the federation we need to setup|Not defined

Defining the federation upstream configuration:

```yaml
rabbitmq_federation_upstream:
  - name: name_of_the_upstream
    vhost: local_vhost_to_federate
    value: json_value
```

See the [RabbitMQ documentation](http://www.rabbitmq.com/federation.html) for
the possible JSON value.

## Files required

You have to put the needed certificates in your `files/` folder:

    files/
     |- rabbitmq_{{ rabbitmq_cacert }}.pem
     |- rabbitmq_{{ rabbitmq_server_key }}.pem
     |- rabbitmq_{{ rabbitmq_server_cert }}.pem

## License

BSD
