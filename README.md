# Kubernetes setup using Vagrant and Ansible

This will create 3 VMs using virtualbox, all running Ubuntu 26.04 LTS. One control node and 2 compute nodes,
then it will run ansible playbooks to configure a kubernetes cluster on them.

## Pre-Requisites

**NOTE**: *I run Ubuntu 24.04 on my host machine, so this is geared toward that platform*

* [Install Virtualbox](https://www.oracle.com/virtualization/technologies/vm/downloads/virtualbox-downloads.html)
* [Install Ansible](https://docs.ansible.com/projects/ansible/latest/installation_guide/intro_installation.html)
* Install needed ansible packages:
```bash
ansible-galaxy collection install -r requirements.yml
```

* 12GB free memory
* 12 CPUs
* 60GB disk space

*You can also modify these settings in the Vagrantfile if you want more or less resources,
but keep note of* [these guidelines](https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/create-cluster-kubeadm/#before-you-begin)

## How to create

This takes 10-15 min to complete.
*If its your first time, it may take longer to download the base image.*

```bash
vagrant up
```

## How to destroy

```bash
vagrant destroy -f
```

### Documentation

#### Swap

[Why noswap?](https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/install-kubeadm/#swap-configuration)

#### Containerd

[Install containerd](https://containerd.io/docs/2.2/getting-started/#option-2-from-apt-get-or-dnf)

[Containerd Docs](https://containerd.io/docs/2.2/)

[config.toml](https://containerd.io/docs/main/man/containerd-config.toml.5/)

[Why enable net.ipv4.ip_forward](https://kubernetes.io/docs/setup/production-environment/container-runtimes/#install-and-configure-prerequisites)

#### Kubernetes

[Bootstrapping clusters with kubeadm](https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/)

[Install kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl-linux/#install-kubectl-binary-with-curl-on-linux)

[Enable auto-completion](https://kubernetes.io/docs/tasks/tools/install-kubectl-linux/#bash)

[kubeadm docs](https://kubernetes.io/docs/reference/setup-tools/kubeadm/)

[kubelet docs](https://kubernetes.io/docs/reference/command-line-tools-reference/kubelet/)

[kubectl docs](https://kubernetes.io/docs/reference/kubectl/)

[Cilium](https://docs.cilium.io/en/stable/gettingstarted/k8s-install-default/#cilium-quick-installation)

#### Vagrant

[Vagrantfile](https://developer.hashicorp.com/vagrant/docs/vagrantfile)

