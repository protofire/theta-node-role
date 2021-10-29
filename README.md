Theta Full Node
=========

Use this ansible role to install docker and docker-compose and run Theta blockchain node with eth rpc adapter in docker containers

Theta rpc is available on port 16888. Eth rpc is available on ports 18888 (http) and 18889 (ws)

Ansible will check and create (if does not exist) `/data` directory and put there a snapshot file and `config.yaml` file. This directory will be mounted to the theta-node container and blockchain data will be stored there

It's a good idea to mount an additional drive (e.g. SSD persistent disk in GCP) to the `/data` directory

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

Usage
-----

## Step 1: Prerequisites

To use this role you will need:
- [Ansible >= 2.5.1](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html)
- An instance to run the node on. See [overview](https://docs.thetatoken.org/docs/guardian-node-overview)
- `/data` directory on the instance to store blockchain data

## Step 2: SSH connection

Make sure you can connect to the instance you're going to run Theta node on via SSH with ssh-key. We recommend using a separate key for ansible deployment

## Step 3: Ansible inventory file

Clone this repository and create a file named `hosts.txt` in repository's root directory. The file should be like this:
```ini
[theta]
theta-node ansible_host=<YOUR_NODE_EXTERNAL_IP> ansible_user=<YOUR_NODE_USER> ansible_ssh_private_key_file=~/.ssh/ansible-key
```

## Step 4: Ansible playbook
Create a file named `theta-node-playbook.yml` in repository's root directory. See an example in this README

Make sure you've placed all the necessary variables in the `theta-node-playbook.yml`. You can find variables description in this README file

After all these steps run:
```bash
ansible-playbook theta-node-playbook.yml
```
Ansible will install docker and its requirements, download latest docker-compose binary, create `docker-compose.yml` and `theta.service` files, download latest theta blockchain snapshot and `config.toml` file, enable and start theta node via systemd

Both theta-node and theta-rpc-adaptor container images that are used here are created and maintained by Protofire. You can find corresponding Dockerfiles in the ./files directory of this role


License
-------

GPLv3

Author Information
------------------

[Protofire.io](https://protofire.io/)
