## Fabric development environment on a single server

This Ansible playbook was written to create simple Hyperledger Fabric networks for the purpose of testing. It installs [fabric-dev-servers](https://github.com/hyperledger/composer-tools/tree/master/packages/fabric-dev-servers) and its requirements on a single machine. Currently this was only tested with `Amazon Linux 2 AMI`.

### Usage

Make sure you have [Ansible installed](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html).

#### Server and SSH access

First, create yourself a virtual server somewhere (e.g. AWS). Ensure to have key based SSH access to it, for example by configuration in `~/.ssh/config`:

```
Host my-host
    Hostname my-host-ip-or-name
    User ec2-user
    IdentityFile ~/.ssh/keypairs/my-keypair.pem
```

#### Setup and configuration

1. Clone this repository: `git clone git@github.com:jverhoelen/hyperledger-fabric-dev-ansible.git && cd hyperledger-fabric-dev-ansible`
2. (Optional) adjust `variables.yml` for configuration. The default variables should already be stable.
3. Create your `hosts` file and insert the host you have access to, e.g. `my-host` from the SSH config

### Provision the server

`ansible-playbook -i ./hosts hyperledger-fabric-dev-ansible-playbook.yml`
