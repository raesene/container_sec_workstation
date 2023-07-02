# Container Security Workstation Playbook

This is a set of playbooks for setting up a container security workstation with common tools for doing container security reviews.

There are six playbooks in the repository

- cli_container_sec_workstation.yml - This has only command line tools installed
- gui_container_sec_workstation.yml - This adds tools that have a GUI but relies on a remote X server being available
- desktop_container_sec_workstation.yml - This installs XFCE4 and xrdp so you can get a remote full desktop.
- wsl_container_sec_workstation.yml - This is designed for installation inside a WSL environment that already has Docker installed (via Docker for Windows) and is running as root
- docker_container_sec_workstation.yml - This is designed for creating Docker images for container security testing
- ec2_container_workstation.yml - This is designed for creating an EC2 instance with the tools installed

## Pre-requisites

- Tested on Ubuntu, may work on other deb based distros
- [Ansible install](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html)
  - Probably best to install via pip
- Change the `user` var in the playbook to your username

## EC2 pre-requisites

- You'll need some ansible galaxy modules for this
  - `ansible-galaxy collection install amazon.aws`
  - `ansible-galaxy collection install maxhoesel.caddy`

## Installation Process - VMs

Once you've got the pre-requisites installed, you can just run

```
ansible-playbook [playbook-YAML-file]
```
and it should setup the machine as needed.

## Installation Process - Docker

Here the goal is to use the playbook to create a container that can be turned into an image and pushed to a Docker Registry for later use.

```bash
ansible-playbook docker_container_sec_workstation.yml
```

After running the playbook, stop the container

```bash
docker stop base
```

Then commit it to an image, here container_sec_image

```bash
docker commit base container_sec_image
```

Then push to a registry of your choosing

## Install process EC2

Make sure to look at all the variable specified in the playbook and adapt them as needed. In particular

- `keypair` - The name of the keypair to use for SSH access
- `security_group` - The name of the security group to use for the EC2 instance
- `subnet_id` - The subnet to use for the EC2 instance. this is in the region file in ansible_vars.

If you want a valid TLS cert and have a DNSimple account then uncomment the relevant lines in the playbook, and set 
- `dnsimple_api_key` to your DNSimple API token.
- `dnsimple_account_email` to the email address associated with your DNSimple account
- `dns_domain` to the domain you want a record created at.

Then uncomment the task for setup of the DNS record in the playbook.

## Tools List - Core

- [Auger](https://github.com/jpbetz/auger)
- [Docker](https://www.docker.com/)
- [eathar](https://github.com/raesene/eathar)
- [etcdctl](https://etcd.io/)
- [gvisor](https://gvisor.dev/)
- [helm](https://helm.sh/)
- [kind](https://kind.sigs.k8s.io/)
- [kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl/)
- [kubens & kubectx](https://github.com/ahmetb/kubectx)
- [kube-ps1](https://github.com/jonmosco/kube-ps1)
- [nmap](https://nmap.org/)
- [regclient](https://github.com/regclient/regclient)
- [rbac-tool](https://github.com/alcideio/rbac-tool)
- [teisteanas](https://github.com/raesene/teisteanas)
- [tocan](https://github.com/raesene/tocan)
- [trivy](https://github.com/aquasecurity/trivy/)
- [whaler](https://github.com/P3GLEG/Whaler)
- [dive](https://github.com/wagoodman/dive)

## Tools List - GUI

If you install the GUI or desktop playbooks then it will add some handy tools which require a GUI.  You can then access the VM from a machine with an X Server (e.g. MobaXterm or XMing on Windows) and access these additional tools

- [firefox](https://www.mozilla.org/en-GB/firefox/new/?redirect_source=firefox-com)
- [octant](https://github.com/vmware-tanzu/octant)
- [Visual Studio Code](https://code.visualstudio.com/) , with the Docker, Kubernetes and YAML plugins

## Tools List - EC2

If you install the EC2 playbook it will setup a [Code-Server](https://github.com/coder/code-server) instance on 443/TCP and then hook-up [Caddy](https://caddyserver.com/) to it to provide a reverse proxy with TLS.

## TODO

- Add More tools
- Ensure all tools from remote sources have signature/checksum checking
- Make the EC2 install more flexible, less hardcoded
