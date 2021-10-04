---
title: 'Homelab part 1: Setup Kubernetes on Windows 10 using multipass and k3s'
date: 2021-09-30T20:53:40-04:00
draft: true
toc: true
images:
tags:
  - untagged
---

## Why?

## Perquisites

- Windows 10 Pro with Hyper-V
- A familiarity with the command line

## Overview

- Enable Hyper-V
- Install Chocolatey and use it to install Multipass (for quick VMs to host a Kubernetes single-node cluster)
- Create Ubuntu VMs with Multipass
- Install a multi-node Kubernetes cluster on the Multipass VMs with k3s (https://k3s.io/)
- Install Helm to use to deploy Rancher dependencies to the cluster
- Deploy Rancher on the k8s cluster to manage it
- Deploy apps via Rancher

## Steps

1. Run multipass to deploy vms (lnx00-03)

```shell
multipass launch -name lnx01
multipass launch -name lnx02
multipass launch -name lnx03
```

2. Get IPs for vms

```shell
multipass list

Name                    State             IPv4             Image
primary                 Running           172.24.201.216   Ubuntu 20.04 LTS
lnx01                   Running           172.24.203.141   Ubuntu 20.04 LTS
lnx02                   Running           172.24.192.148   Ubuntu 20.04 LTS
lnx03                   Running           172.24.204.44    Ubuntu 20.04 LTS
```

3. Add the VMs to the /etc/hosts file of each vm (use `multipass shell vmName` to get access)

```shell
sudo tee -a /etc/hosts<<EOF
172.24.203.141 k3s-master
172.24.192.148 k3s-worker1
172.24.204.44 k3s-worker2
EOF
```

5. Add the VMs to the `C:\Windows\System32\drivers\etc\hosts` file as well.
6. Update each VM while there `sudo apt update && sudo apt -y upgrade `
7. Install k3s on the lnx01 (master node)

```shell
curl -sfL https://get.k3s.io | sh -
```

## Configuration

1. Get token from master `sudo cat /var/lib/rancher/k3s/server/node-token`
2. Set it as a variable along with your preferred url and start the k3s-agent on all three vms

```shell
ubuntu@lnx01:~$ k3s_url="https://k3s-master:6443"
ubuntu@lnx01:~$ k3s_token="K1017e404f267f8f1bfbaa3647a85f657e5542cbb977b1a81976a73eec3af8e636c::server:11a25b3ed501cbc20489082ebb8264fc"
ubuntu@lnx01:~$ curl -sfL https://get.k3s.io | K3S_URL=${k3s_url} K3S_TOKEN=${k3s_token} sh -s - --write-kubeconfig-mode 644
```

3. Repeat step 3 for lnx02 or worker2
4. Back on the master VM, check on the agents

```shell
ubuntu@lnx01:~$ sudo kubectl get nodes
NAME    STATUS   ROLES                  AGE     VERSION
lnx01   Ready    control-plane,master   10m     v1.21.5+k3s1
lnx02   Ready    <none>                 2m21s   v1.21.5+k3s1
lnx03   Ready    <none>                 94s     v1.21.5+k3s1
```

5. Install Helm

vaai
