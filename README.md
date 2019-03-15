## Fabric development environment on a single server

This Ansible playbook was written to create simple Hyperledger Fabric networks for the purpose of testing. It installs [fabric-dev-servers](https://github.com/hyperledger/composer-tools/tree/master/packages/fabric-dev-servers) and its requirements on a single machine. Until now this was tested with Amazon Linux 2 AMI and Ubuntu 18.04.1 LTS (using Vagrant).

### Usage

#### Prerequisite

Make sure you have [Ansible](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html) and [composer-cli](https://hyperledger.github.io/composer/latest/) installed.

If you just want to test this setup, feel free to use Vagrant. This repository comes with a suitable `Vagrantfile`, start the machine using `vagrant up`. Once started, run `vagrant ssh` to connect to the machine initially. Then append the machines ssh-config to `~/.ssh/config` if you like:

```bash
vagrant ssh-config >> ~/.ssh/config
```

You should now be able to run `ssh ubuntu-vagrant`. `ubuntu-vagrant` now is the host you can use in the hosts-file (skip the sections of this tutorial until you get to "Setup and configuration"). The public IP, available from your host machine, will be `192.168.50.4`.

#### Server and SSH access

First, create yourself a virtual server somewhere (e.g. AWS EC2). Ensure to have key based SSH access to it, for example by configuration in `~/.ssh/config`:

```
Host my-host
    Hostname my-host-ip-or-name
    User ec2-user
    IdentityFile ~/.ssh/keypairs/my-keypair.pem
```

#### Inbound traffic

Make sure to allow inbound traffic to the server (e.g. via a AWS Security Group) to the following ports:

* 8080 – Composer Playground
* 7054 – ca.org1
* 7051 and 7053 – peer0.org1
* 7050 – Orderer
* SSH port of your choice
* Maybe additional ones for different [composer-rest-server](https://hyperledger.github.io/composer/latest/integrating/integrating-index) instances


#### Setup and configuration

1. Clone this repository: `git clone git@github.com:jverhoelen/hyperledger-fabric-dev-ansible.git && cd hyperledger-fabric-dev-ansible`
2. (Optional) adjust `variables.yml` for configuration. The default variables should already be stable.
3. Create your `hosts` file and insert the host you have access to, e.g. `my-host` from the SSH config

### Provision the server

Let's go:

`ansible-playbook -i ./hosts playbook.yml`

*Note*: On the first run(s) there can be an issue with task `Download fabric docker images by script`. It may complain about your user not having sufficient rights for Docker although the user has been added to the `docker` group before. **Just run it again**. If is takes too long, you can also consider to login manually and execute `~/fabric-tools/downloadFabric.sh`.
