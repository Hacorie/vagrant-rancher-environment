# Rancher Vagrant

Vagrant file for setting up a Rancher environment. Inspired by examples
from [vagrant-rancher](https://github.com/nextrevision/vagrant-rancher)

### Prerequisities
  - __Virtual Box__: [https://www.virtualbox.org/wiki/Downloads]()
  - __Vagrant__: [https://www.vagrantup.com/downloads.html]()
  - __rancher-vagrant plugin__: ```vagrant plugin install vagrant-rancher```

### What's installed

The following VMs are created by the ```Vagrantfile```

| Name | IP  | RAM (GB)  | Purpose  |
|---|---|---|---|---|
| rancher-server  | 192.168.33.100 | 1.5 | Hosts the rancher-server container (with no agent) |
| rancher-agent1 | 192.168.33.101 | 1.0 | Hosts the rancher-agent container |
| rancher-agent2 | 192.168.33.102 | 1.0 | Hosts the rancher-agent container |
| rancher-agent3 | 192.168.33.103 | 1.0 | Hosts the rancher-agent container |

__note 1__: networking is set to ```host only```

__note 2__: RAM requirements are for Rancher to run and for the agents to support database containers with
certain RAM requirements (MySQL, etc)

### Sync Folders

The __/vagrant__ directory on each machine is mapped to __/data/\<machine-hostname>/__ on the host


### Usage

- ```vagrant up``` :]
- access the Rancher server via [http://192.168.33.100:8080/]()
