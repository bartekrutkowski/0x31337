# 0x31337
**Technical DevOps demo for source control, automation orchestration and sotware configuration management**

This repository contains a technical demo for a given task of:
- Launch 3 separate Linux nodes using the tool/distro of your choice
  - 2x application nodes
  - 1x web node
- Using either Chef or Ansible deliver following
  - Deploy the sample application to application nodes
  - Install Nginx on the web node and balance requests to the application nodes in round-robin fashion
  - Demonstrate the round-robin mechanism is working correctly
- Invocation should be a one line command string
- Take care not to over engineer the solution
- Bonus points: for changes to the sample code, automate the build and delivery to the environment

This repository contains the complete solution that was build keeping the simplicity requirement as main priority. It does not aim to be-all-end-all solution for this problem.


# Requirements

To deploy and test this solution one needs a host machine with the following software installed:

- Vagrant 1.8.x
- Oracle VirtualBox 5.0.x
- Ansible 2.0.x
- The ansible-galaxy module 'geerlingguy.nginx' 1.8.x


# Usage

Clone the repository into proper location:

```sh
$ git clone https://github.com/bartekrutkowski/0x31337.git ~/
```

Launch the vagrant provisioning:

```sh
$ cd ~/0x31337
$ vagrant up
```

Observe the vm's being provisioned, configured with Ansible, and enjoy the roundrobin loadbalancing demonstration at the end:

```sh
[... output stripped ...]
PLAY RECAP *********************************************************************
web01                      : ok=14   changed=9    unreachable=0    failed=0

==> web01: Running provisioner: shell...
    web01: Running: inline script
==> web01: Hi there, I'm served from app01!
==> web01: Hi there, I'm served from app02!
==> web01: Hi there, I'm served from app01!
==> web01: Hi there, I'm served from app02!
$
```

In case we'd want to change the application source and redeploy:

```sh
$ vi ansible/roles/app/files/app.go                                            
$ vagrant provision
[... output stripped ...]
PLAY RECAP *********************************************************************
web01                      : ok=13   changed=0    unreachable=0    failed=0

==> web01: Running provisioner: shell...
    web01: Running: inline script
==> web01: app01 says 31337 h3110!
==> web01: app02 says 31337 h3110!
==> web01: app01 says 31337 h3110!
==> web01: app02 says 31337 h3110!
$
```

To clean up after observing this wonder of DevOps technology:

```sh
$ vagrant destroy -f
$ cd
$ rm -rf ~/0x31337
```

You're done!


# Epilogue

Given the requirement were not mentioning any particular host/cloud platform to be used and asked for simplicity of the solution at the same time, the choice of Vagrant and VirtualBox was obvious considering today's technology. Shall this require deployment demonstration on one of available mainstream cloud platforms (like AWS or GCE) other tools could, and should, be used, like Terraform or Ansible for vm's provisioning.

The configuration management Ansible code also isnt in production ready shape: the modules logic should not contain hardoded paths, IP addresses, the application code could be stored in external repository and fetched, and so on, and so fort. All imperfections in this code are actually somewhat intentional, following the mantra of 'make it work, make it right, make it fast' - the first part is considered to be enough for the purpose of this demonstration.


# Additional resources

- <https://www.vagrantup.com/docs/installation/>
- <http://docs.ansible.com/ansible/intro_installation.html>
  - <http://docs.ansible.com/ansible/galaxy.html>
- <http://nginx.org/en/docs/http/load_balancing.html>
- <https://golang.org/doc/>
- <http://www.urbandictionary.com/define.php?term=31337>
