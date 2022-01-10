---
title: 'How to run Jenkins locally with Kind'
date: 2021-11-23T21:47:30-05:00
draft: true
toc: true
cover: /img/jenkins.png
tags:
  - kind
  - kubernetes
  - homelab
  - jenkins
---

## Why?

## Perquisites

- [Go installed](https://go.dev/doc/install)
- [Chocolatey installed](https://chocolatey.org/)

Note: I'm using Windows 10 Pro, you will need to adapt this if you are running something else

## Steps

### 1. Install Docker-Desktop

Install it with:

```powershell
choco install docker-desktop
```

And start it up

```powershell
Start-Process 'C:\Program Files\Docker\Docker\Docker Desktop.exe'
```

### Install kubectl

[Instructions](https://kubernetes.io/docs/tasks/tools/install-kubectl-windows/#install-on-windows-using-chocolatey-or-scoop)

```powershell
choco install kubernetes-cli
```

```powershell
cd ~
```

```powershell
mkdir .kube
```

```powershell
cd .kube
```

```powershell
New-Item config -type file
```

If you are using

### Install kind

```powershell
choco install kind
```

### Create multi-node cluster

git clone

```powershell
kind create cluster --config .\multi-node.yaml
```

### Install nginx ingress

??

```powershell
kubectl apply --filename https://raw.githubusercontent.com/kubernetes/ingress-nginx/master/deploy/static/provider/kind/deploy.yaml
```

HELM jeknisn
`helm repo add jenkinsci https://charts.jenkins.io`
`helm repo udpate`
`helm upgrade --install jenkins jenkinsci/jenkins --namespace jenkins --create-namespace --values jenkins-values.yaml --wait`

## Conclusion
