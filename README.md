Theta Full Node
=========

Use this ansible role to install docker and docker-compose and run Theta blockchain node with eth rpc adapter in docker containers container

Theta rpc is available on port 16888. Eth rpc is available on ports 18888 (http) and 18889 (ws)

Also Ansible will check and create (if does not exist) `/data` directory and put there a snapshot file and `config.yaml` file. This directory will be mounted to the theta-node container and blockchain data will be stored there

It's a good idea to mount some additional drive (e.g. SSD persistent disk in GCP) to the `/data` directory

Requirements
------------

Debian or Red Hat OS family

Role Variables
--------------

| Variable | Description | Default value |
| :------- | :---------- | :------------ |
| theta_snapshot_url | URL that contains another URL to download snapshot from | https://mainnet-data.thetatoken.org/snapshot |
| download_snapshot  | Set `true` to start a node from snapshot                | true                                         |
| theta_node_image_tag | Docker image tag for theta node                       | "3.1.2" |
| theta_rpc_image_tag  | Docker image tag for theta eth rpc adaptor            | "0.0.1" |
| theta_nat_mapping    | Set `true` if the node is behind NAT                  | false   |
| theta_node_password  | A password to launch Theta node. Must be set          |         |

Dependencies
------------

None

Example Playbook
----------------

```yaml
- hosts: theta-node-gcp
  roles:
      - role: theta-node
        vars: 
          theta_node_password: "Pa$$w0rd"
          theta_nat_mapping: true

```

License
-------

GPLv3

Author Information
------------------

[Protofire.io](https://protofire.io/)
