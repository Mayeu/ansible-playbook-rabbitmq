# Rabbitmq Playbook

Playbook to install and configure rabbitmq. Will come with various
configuration tweaking later on.

## Supported system

Currently only Debian Jessie amd64 is tested. Patch welcome to support other
OS.

## Installation

Just clone (or submodule) this repository under the name `rabbitmq` in your
`roles` directory. This file, `test.yml` and `Vagrantfile` will be ignored by
ansible anyway.

## Using it

### Variables


|Name|Type|Description|Default|
|----|----|-----------|-------|
`rabbitmq_conf_tcp_listeners_address`|String|listening address for the tcp interface|`''`
`rabbitmq_conf_tcp_listeners_port`|Integer|listening port for the tcp interface|`5672`
`rabbitmq_conf_ssl_listeners_address`|String|listening address for the ssl interface|'0.0.0.0'`
`rabbitmq_conf_ssl_listeners_port`|Integer|listening port for the ssl interface|`5671`
`rabbitmq_conf_ssl_options_cacertfile`|String|Path the CA certificate|`"/etc/rabbitmq/ssl/cacert.pem"`
`rabbitmq_conf_ssl_options_certfile`|String|Path to the server certificate|`"/etc/rabbitmq/ssl/server_cert.pem"`
`rabbitmq_conf_ssl_options_keyfile`|String|Path to the private key file|`"/etc/rabbitmq/ssl/server_key.pem"`
`rabbitmq_vhost_definitions`|List of hash|Define the vhost, and associated users and password (see below)|Not defined

Defining the vhost configuration:

```yaml
rabbitmq_vhost_definitions:
  - vhost   : vhost1
    user    : user1
    password: password1
  - vhost   : vhost2
    user    : user2
    password: password2
```


### Files

You have to put the needed certificates in your `files/` folder:

    files/
     |- rabbitmq_cacert.pem
     |- rabbitmq_server_cert.pem
     |- rabbitmq_server_key.pem

## Test

There is some really basic tests with the playbook. It just try to install
rabbitmq in a vagrant VM. Just run:

    $ vagrant up

and the VM will be provisioned by ansible with the test.yml.
