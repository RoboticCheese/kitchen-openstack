[![Gem Version](https://badge.fury.io/rb/kitchen-openstack.png)](http://badge.fury.io/rb/kitchen-openstack)
[![Build Status](https://travis-ci.org/test-kitchen/kitchen-openstack.png?branch=master)](https://travis-ci.org/test-kitchen/kitchen-openstack)
[![Code Climate](https://codeclimate.com/github/test-kitchen/kitchen-openstack.png)](https://codeclimate.com/github/test-kitchen/kitchen-openstack)
[![Coverage Status](https://coveralls.io/repos/test-kitchen/kitchen-openstack/badge.png)](https://coveralls.io/r/test-kitchen/kitchen-openstack)
[![Dependency Status](https://gemnasium.com/test-kitchen/kitchen-openstack.png)](https://gemnasium.com/test-kitchen/kitchen-openstack)

# Kitchen::OpenStack

An OpenStack Nova driver for Test Kitchen 1.0!

Shamelessly copied from [Fletcher Nichol](https://github.com/fnichol)'s
awesome work on an [EC2 driver](https://github.com/opscode/kitchen-ec2).

## Installation

Add this line to your application's Gemfile:

    gem 'kitchen-openstack'

And then execute:

    $ bundle

Or install it yourself as:

    $ gem install kitchen-openstack

## Usage

Provide, at a minimum, the required driver options in your `.kitchen.yml` file:

    driver:
      name: openstack
      openstack_username: [YOUR OPENSTACK USERNAME]
      openstack_api_key: [YOUR OPENSTACK API KEY]
      openstack_auth_url: [YOUR OPENSTACK AUTH URL]
      require_chef_omnibus: [e.g. 'true' or a version number if you need Chef]
      image_ref: [SERVER IMAGE ID]
      flavor_ref: [SERVER FLAVOR ID]

The `image_ref` and `flavor_ref` options can be specified as an exact id,
an exact name, or as a regular expression matching the name of the image or flavor.

By default, a unique server name will be generated and the current user's SSH
key will be used (with an RSA key taking precedence over a DSA), though that
behavior can be overridden with additional options:

    server_name: [A UNIQUE SERVER NAME]
    server_name_prefix: [STATIC PREFIX FOR RANDOM SERVER NAME]
    private_key_path: [PATH TO YOUR PRIVATE SSH KEY]
    public_key_path: [PATH TO YOUR SSH PUBLIC KEY]
    username: [SSH USER]
    port: [SSH PORT]
    key_name: [SSH KEY NAME]
    openstack_tenant: [YOUR OPENSTACK TENANT ID]
    openstack_region: [A VALID OPENSTACK REGION]
    availability_zone: [AN OPENSTACK AVAILABILITY ZONE]
    openstack_service_name: [YOUR OPENSTACK COMPUTE SERVICE NAME]
    openstack_network_name: [YOUR OPENSTACK NETWORK NAME USED TO CONNECT]
    security_groups:
      - [A LIST OF...]
      - [...SECURITY GROUPS TO JOIN]
    network_ref:
      - [OPENSTACK NETWORK NAMES OR...]
      - [...ID TO CREATE INSTANCE WITH]
    no_ssh_tcp_check: [DEFAULTS TO false, SKIPS TCP CHECK WHEN true]
    no_ssh_tcp_check_sleep: [NUM OF SECONDS TO SLEEP IF no_ssh_tcp_check IS SET]
    use_volume_store: [DEFAULTS TO false, IF ture OPENSTACK WILL USE VOLUME STORAGE]
    make_new_volume: [DEFAULTS TO false, MAKE A NEW VOLUME BASED ON image_ref WHEN TRUE]
    volume_snapshot: [WHEN make_new_volume AND THIS OPTION ARE TRUE, THE VOLUME WILL BE MADE FROM A VOLUME SNAPSHOT]
    snapshot_id: [SNAPSHOT REF ID]
    volume_size: [THE SIZE OF THE VOLUME IN GB, DEFAULTS TO 20]
    volume_id: [THE VOLUME TO BE USED WHEN make_new_volume IS FALSE]
    volume_device_name: [THE DEVICE NAME TO MOUNT AS, DEFAULTS TO vda]
    delete_volume: [DEFAULTS TO false, WHEN true THE VOLUME WILL BE DELETED ON DESTROY]

If a `server_name_prefix` is specified then this prefix will be used when 
generating random names of the form `<NAME PREFIX>-<RANDOM STRING>` e.g.
`myproject-asdfghjk`. If both `server_name_prefix` and `server_name` are 
specified then the `server_name` takes precedence.

If a `key_name` is provided it will be used instead of any
`public_key_path` that is specified.

If a `key_name` is provided without any `private_key_path`, unexpected
behavior may result if your local RSA/DSA private key doesn't match that
OpenStack key.

A specific `floating_ip` or the ID of a `floating_ip_pool` can be provided to
bind a floating IP to the node. Any floating IP will be the IP used for
Test Kitchen's SSH calls to the node.

    floating_ip: [A SPECIFIC FLOATING IP TO ASSIGN]
    floating_ip_pool: [AN OPENSTACK POOL NAME TO ASSIGN THE NEXT IP FROM]

The `network_ref` option can be specified as an exact id, an exact name,
or as a regular expression matching the name of the network. You can pass one

    network_ref: MYNET1

or many networks

    network_ref:
      - MYNET1
      - MYNET2

The `openstack_network_name` is used to select IP address for SSH connection.
It's recommended to specify this option in case of multiple networks used for
instance to provide more control over network connectivity.

Please note that `network_ref` relies on Network Services (`Fog::Network`) and
it can be unavailable in your OpenStack installation.

    disable_ssl_validation: true

Only disable SSL cert validation if you absolutely know what you are doing,
but are stuck with an OpenStack deployment without valid SSL certs.

## Contributing

1. Fork it
2. Create your feature branch (`git checkout -b my-new-feature`)
3. Run style checks and RSpec tests (`bundle exec rake`)
4. Commit your changes (`git commit -am 'Add some feature'`)
5. Push to the branch (`git push origin my-new-feature`)
6. Create new Pull Request
