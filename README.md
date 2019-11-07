# Container Security Workstation Playbook

This is a playbook for setting up a container security workstation with common tools for doing container security reviews.

Rough draft at the moment, still quite a bit of work to do.

## Pre-requisites

- Tested on Ubuntu, may work on other deb based distros
- [Ansible install](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html)
- Change the `user` var in the playbook to your username

## Tools List

- [Auger](https://github.com/jpbetz/auger)
- [Docker](https://www.docker.com/)
- [Docker Compose](https://docs.docker.com/compose/)
- [etcdctl](https://etcd.io/)
- [gvisor](https://gvisor.dev/)
- [helm](https://helm.sh/)
- [kind](https://kind.sigs.k8s.io/)
- [kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl/)
- [kubens & kubectx](https://github.com/ahmetb/kubectx)
- [kube-ps1](https://github.com/jonmosco/kube-ps1)
- [nmap](https://nmap.org/)
- [rakkess](https://github.com/corneliusweig/rakkess)
- [kubectl-who-can](https://github.com/aquasecurity/kubectl-who-can)
- [rback](https://github.com/team-soteria/rback)
- [trivy](https://github.com/aquasecurity/trivy/)
- [whaler](https://github.com/P3GLEG/Whaler)
- [dive](https://github.com/wagoodman/dive)

## TODO

- Add More tools
  - kube-bench
- Make installation patterns consistent
- Check Checksums/signatures where apt not used