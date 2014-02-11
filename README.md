# Rabbitmq Playbook

Playbook to install and configure rabbitmq. Will come with various
configuration tweaking later on.

If you wish to discuss modifications, or help to support more plateforms, open
an issue.

## Supported system

Currently only Debian Jessie and Wheezy on amd64 is supported. Patch welcome to
support other OS.

### Role Variables

|Name|Type|Description|Default|
|----|----|-----------|-------|
`rabbitmq_cacert`|String|Name of the CA certificate file. Will be prefixed by `rabbitmq_` and postfixed by `.pem`|`cacert`
`rabbitmq_server_key`|String|Name of the SSL key file. Will be prefixed by `rabbitmq_` and postfixed by `.pem`|`server_key`
`rabbitmq_cert_key`|String|Name of the SSL certificate file. Will be prefixed by `rabbitmq_` and postfixed by `.pem`|`server_cert`
`rabbitmq_conf_tcp_listeners_address`|String|listening address for the tcp interface|`''`
`rabbitmq_conf_tcp_listeners_port`|Integer|listening port for the tcp interface|`5672`
`rabbitmq_conf_ssl_listeners_address`|String|listening address for the ssl interface|'0.0.0.0'`
`rabbitmq_conf_ssl_listeners_port`|Integer|listening port for the ssl interface|`5671`
`rabbitmq_conf_ssl_options_cacertfile`|String|Path the CA certificate|`"/etc/rabbitmq/ssl/cacert.pem"`
`rabbitmq_conf_ssl_options_certfile`|String|Path to the server certificate|`"/etc/rabbitmq/ssl/server_cert.pem"`
`rabbitmq_conf_ssl_options_keyfile`|String|Path to the private key file|`"/etc/rabbitmq/ssl/server_key.pem"`
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

## Files required

You have to put the needed certificates in your `files/` folder:

    files/
     |- rabbitmq_{{ rabbitmq_cacert }}.pem
     |- rabbitmq_{{ rabbitmq_server_key }}.pem
     |- rabbitmq_{{ rabbitmq_server_cert }}.pem

## License

BSD
