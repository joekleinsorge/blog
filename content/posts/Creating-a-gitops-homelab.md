---
title: 'Creating a Gitops Homelab'
date: 2022-02-07T21:18:00-05:00
draft: true
toc: true
cover:
tags:
  - gitops
  - homelab
---

## Objectives

Because it always pays to have the end in mind before you begin.

### Primary

My primary objective for my homelab has always been for a way to learn new technologies and to play with the shiny new stuff.

### Secondary

Second to that would be providing services to my house and family ( Reliable WiFi, Plex, etc. )

`GitOps` is the "new" idea to chase in the tech world. The idea of having one source controlled source of truth for everything is appealing to anyone who has worked in IT operations for a variety of reasons. Now that IaC is widely adopted, it just makes sense to keep everything needed to deploy your service in a vsc.

- Single Source of Truth:- The first and most important principle of GitOps. It states Git is always right. You can understand the whole system just by looking at Git because it has all the ingredients right there.
- Everything as a Code:- It states everything should be kept in the form of code be it the application or any other component which application needed. In most cases, infrastructure is also defined in the form of code. For example:- Cloud VMs, Docker containers, Kubernetes Deployments.
- CI/CD Automation:- This is the feature where the magic happens. Build, Test and Deploy will happen automatically relying on the truth in the repository. Infrastructure creation and application deployment will be part of this according to the infrastructure and configuration as a code.

## Tech stack

<table>
  <tr>
    <th>Logo</th>
    <th>Name</th>
    <th>Description</th>
  </tr>
  <tr>
    <td><img width="32" src="https://simpleicons.org/icons/ansible.svg"></td>
    <td><a href="https://www.ansible.com">Ansible</a></td>
    <td>Automate bare metal provisioning and configuration</td>
  </tr>
  <tr>
    <td><img width="32" src="https://cncf-branding.netlify.app/img/projects/argo/icon/color/argo-icon-color.svg"></td>
    <td><a href="https://argoproj.github.io/cd">ArgoCD</a></td>
    <td>GitOps tool built to deploy applications to Kubernetes</td>
  </tr>
  <tr>
    <td><img width="32" src="https://github.com/jetstack/cert-manager/raw/master/logo/logo.png"></td>
    <td><a href="https://cert-manager.io">cert-manager</a></td>
    <td>Cloud native certificate management</td>
  </tr>
  <tr>
    <td><img width="32" src="https://avatars.githubusercontent.com/u/314135?s=200&v=4"></td>
    <td><a href="https://www.cloudflare.com">Cloudflare</a></td>
    <td>DNS and Tunnel</td>
  </tr>
  <tr>
    <td><img width="32" src="https://www.docker.com/sites/default/files/d8/2019-07/Moby-logo.png"></td>
    <td><a href="https://www.docker.com">Docker</a></td>
    <td>Ephermeral PXE server and convenient tools container</td>
  </tr>
  <tr>
    <td><img width="32" src="https://upload.wikimedia.org/wikipedia/commons/b/bb/Gitea_Logo.svg"></td>
    <td><a href="https://gitea.com">Gitea</a></td>
    <td>Self-hosted Git service</td>
  </tr>
  <tr>
    <td><img width="32" src="https://grafana.com/static/img/menu/grafana2.svg"></td>
    <td><a href="https://grafana.com">Grafana</a></td>
    <td>Operational dashboards</td>
  </tr>
  <tr>
    <td><img width="32" src="https://cncf-branding.netlify.app/img/projects/helm/icon/color/helm-icon-color.svg"></td>
    <td><a href="https://helm.sh">Helm</a></td>
    <td>The package manager for Kubernetes</td>
  </tr>
  <tr>
    <td><img width="32" src="https://cncf-branding.netlify.app/img/projects/k3s/icon/color/k3s-icon-color.svg"></td>
    <td><a href="https://k3s.io">K3s</a></td>
    <td>Lightweight distribution of Kubernetes</td>
  </tr>
  <tr>
    <td><img width="32" src="https://cncf-branding.netlify.app/img/projects/kubernetes/icon/color/kubernetes-icon-color.svg"></td>
    <td><a href="https://kubernetes.io">Kubernetes</a></td>
    <td>Container-orchestration system, the backbone of this project</td>
  </tr>
  <tr>
    <td><img width="32" src="https://github.com/grafana/loki/blob/main/docs/sources/logo.png?raw=true"></td>
    <td><a href="https://grafana.com/oss/loki">Loki</a></td>
    <td>Log aggregation system</td>
  </tr>
  <tr>
    <td><img width="32" src="https://cncf-branding.netlify.app/img/projects/longhorn/icon/color/longhorn-icon-color.svg"></td>
    <td><a href="https://longhorn.io">Longhorn</a></td>
    <td>Cloud native distributed block storage for Kubernetes</td>
  </tr>
  <tr>
    <td><img width="32" src="https://avatars.githubusercontent.com/u/60239468?s=200&v=4"></td>
    <td><a href="https://metallb.org">MetalLB</a></td>
    <td>Bare metal load-balancer for Kubernetes</td>
  </tr>
  <tr>
    <td><img width="32" src="https://avatars.githubusercontent.com/u/1412239?s=200&v=4"></td>
    <td><a href="https://www.nginx.com">NGINX</a></td>
    <td>Kubernetes Ingress Controller</td>
  </tr>
  <tr>
    <td><img width="32" src="https://cncf-branding.netlify.app/img/projects/prometheus/icon/color/prometheus-icon-color.svg"></td>
    <td><a href="https://prometheus.io">Prometheus</a></td>
    <td>Systems monitoring and alerting toolkit</td>
  </tr>
  <tr>
    <td><img width="32" src="https://avatars.githubusercontent.com/u/75713131?s=200&v=4"></td>
    <td><a href="https://rockylinux.org">Rocky Linux</a></td>
    <td>Base OS for Kubernetes nodes</td>
  </tr>
  <tr>
    <td><img width="32" src="https://avatars.githubusercontent.com/u/47602533?s=200&v=4"></td>
    <td><a href="https://tekton.dev">Tekton</a></td>
    <td>Cloud native solution for building CI/CD systems</td>
  </tr>
  <tr>
    <td><img width="32" src="https://trow.io/trow.png"></td>
    <td><a href="https://trow.io">Trow</a></td>
    <td>Private container registry</td>
  </tr>
  <tr>
    <td><img width="32" src="https://simpleicons.org/icons/vault.svg"></td>
    <td><a href="https://www.vaultproject.io">Vault</a></td>
    <td>Secrets and encryption management system</td>
  </tr>
</table>

## Infrastructure

While you could run your homelab in the cloud, I prefer to keep my services and lab close by to tinker with. I've been burned once before by a larger than expected AWS, bill due to something I forgot I provisioned. With on-prem, all I have to expect is a steady electric bill and the cost of components when I feel like upgrading.

- Compute
  - 3 × USFF PCs `Dell 3080 Micro` :
    - CPU: `Intel Core i3-10100T 3.00GHz`
    - RAM: `4GB`
    - HDD: `500GB 7200RPM`
- Storage
  - 1 x Synology `DS1817+`
    - HDD: `25TB` ( `41TB raw` )
    - SSD: `250GB`
- Network

  - 1 x US-24 Switch
  - 1 x UniFi Security Gateway

    | Architecture            |
    | ----------------------- |
    | ./apps                  |
    | ----------------------- |
    | ./platform              |
    | ----------------------  |
    | ./system + ./external   |
    | ----------------------  |
    | ./bootstrap             |
    | ----------------------- |
    | ./metal                 |
    | ----------------------- |
    | hardware                |
    | ----------------------- |

## Prerequisites

- Gather the MAC addresses of your nodes
  - node1
    - b0:4f:13:06:82:1d
    - port 11
  - node2
    - b0:4f:13:06:08:33
    - port 14
  - node3
    - b0:4f:13:06:82:ee
    - port 15

## Steps

- Create a linux VM on the synology
- Create inv file?
- Run ansible playbooks
  - boot
  - cluster
  - shutdown
  - Use Docker-compose to build:
    - PXE Server
    - DHCP
    - TFTP
    - HTTP
  - Build the ./metal layer:
  - Create an ephemeral, stateless PXE server
  - Install Linux on all servers in parallel
  - Build a Kubernetes cluster (based on k3s)
- Build the ./bootstrap layer:
  - Install ArgoCD
  - Install ApplicationSet to manage other layers (and also manage itself)
- From now on, ArgoCD will do the rest
  - Build the ./system layer (storage, networking, monitoring, etc)
  - Build the ./platform layer (Gitea, Vault, SSO, etc)
  - Build the ./apps layer: (Syncthing, Jellyfin, etc)
- Ansible renders the configuration file for each bare metal machine (like IP, hostname...) and the PXE server from templates
- The tools container creates sibling containers to build a PXE server (includes DHCP, TFTP and HTTP server)
- Ansible wake the machines up using Wake on LAN
- The machine start the boot process, the OS get installed (through PXE server) and the machine reboots to the new operating system
- Ansible build a Kubernetes cluster based on k3s

## Actual Steps

- Prepare starter VM
  - Rocky 8.5 on hyper-v
  - dnf install git
  - dnf install ansible
  - dnf install docker
  - dnf install python3.10
  - dnf install xorriso.x86_64
  - pip3.6 install netaddr
  - pip3.6 install ipaddr
  - pip3.6 install docker
  - pip3.6 install docker-compose
  - Then decide to not deal with pxe booting because I dislike it
  - Create ssh key on vm
  - scp pub key to nodes
  - create .ssh/authorized_keys
  - `cat id_ed25519.pub >>.ssh/authorized_keys`
  - install kubectl
  - ansible-galaxy collection install kubernetes.core
  - pip3.6 install kubernetes
  -

## Setup Ansible host

  - dnf install python3.10
