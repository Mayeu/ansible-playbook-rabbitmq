#Rabbitmq Playbook

(really) Simple playbook to install and configure rabbitmq. Will come with
various configuration tweaking later on :)

##Supported system

Currently only Debian Jessie amd64 is supported and tested. Patch welcome to
support other OS.

##Installation

Just clone (or submodule) this repository under the name `rabbitmq` in your
`roles` directory. This file, `test.yml` and `Vagrantfile` will be ignored by
ansible anyway.

###Using it

You have to put the needed certificates in your `files/` folder, those will be
used to encrypt the communication between your different host:

    files/
     |- rabbitmq_cacert.pem
     |- rabbitmq_server_cert.pem
     |- rabbitmq_server_key.pem

Please note that the `files` folder of this repository contains dummy
certificate. You should **not** use them. Your certificate will get precedence
over the ones of this role if put in your root `files` folder.

##Test

There is some really basic tests with the playbook. It just try to install
rabbitmq in a vagrant VM. Just run:

    $ vagrant up

and the VM will be provisioned by ansible with the test.yml.
