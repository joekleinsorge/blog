---
title: 'Using Terraform to build with Kubernetes on Docker-Desktop'
date: 2021-09-27T19:49:23-04:00
draft: true
images:
tags:
  - Docker
  - Docker-Desktop
  - Terraform
  - Kubernetes
  - Homelab
---

{{< figure src="https://images.unsplash.com/photo-1613690399151-65ea69478674?ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&ixlib=rb-1.2.1&auto=format&fit=crop&w=1173&q=80">}}

## Why?

### about Terraform

[link](https://www.terraform.io/)

### about Docker-Desktop

[link](https://docs.docker.com/desktop/)

### about Kubernetes

[link](https://kubernetes.io/docs/tutorials/kubernetes-basics/)

## Perquisites

I'll be doing all this on a Windows 10 Pro PC. If you don't have them already we will need to install a few things.

### Install Chocolatey

Open a PowerShell terminal as an Admin and run the below code. If you want to read the docs, look [here](https://chocolatey.org/install).

```PowerShell
Set-ExecutionPolicy Bypass -Scope Process -Force; [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072; iex ((New-Object System.Net.WebClient).DownloadString('https://community.chocolatey.org/install.ps1'))
```

### Install Docker-Desktop

Installing everything else is super easy now. In the same terminal run the following code to install [Docker-Desktop](https://www.docker.com/products/docker-desktop).

```PowerShell
choco install docker-desktop
```

### Install Helm

We are going to do the same with [Helm](https://helm.sh/docs/intro/install/)

```PowerShell
choco install kubernetes-helm
```

### Install terraform

Anddd again with Terraform, isn't this nicer than clicking through a install wizard and hoping you didn't agree to selling your soul?

```PowerShell
choco install terraform
```

I won't go over adding it to your PATH, but here is the ALIASes I use for it in my $PROFILE.

```PowerShell
New-Alias -Name 'tf' -Value 'terraform'
function Invoke-terraformFormatRecursive { terraform fmt -recursive }
Set-Alias -Name 'tff' -Value Invoke-terraformFormatRecursive
```

### Enable Kubernetes

Head into the Docker-Desktop settings and flip the switch to enable kubernetes.

## Getting Started

### Create Helm Chart

Create a Helm Chart by running:

```PowerShell
helm create demo
```

### Build main.tf

```terraform
terraform {
  required_providers {
    kubernetes = {
      source  = "hashicorp/kubernetes"
      version = ">= 2.0.0"
    }
  }
}
provider "kubernetes" {
  config_path = "~/.kube/config"
}
resource "kubernetes_namespace" "test" {
  metadata {
    name = "nginx"
  }
}
resource "kubernetes_deployment" "test" {
  metadata {
    name      = "nginx"
    namespace = kubernetes_namespace.test.metadata.0.name
  }
  spec {
    replicas = 2
    selector {
      match_labels = {
        app = "MyTestApp"
      }
    }
    template {
      metadata {
        labels = {
          app = "MyTestApp"
        }
      }
      spec {
        container {
          image = "nginx"
          name  = "nginx-container"
          port {
            container_port = 80
          }
        }
      }
    }
  }
}
resource "kubernetes_service" "test" {
  metadata {
    name      = "nginx"
    namespace = kubernetes_namespace.test.metadata.0.name
  }
  spec {
    selector = {
      app = kubernetes_deployment.test.spec.0.template.0.metadata.0.labels.app
    }
    type = "NodePort"
    port {
      node_port   = 30201
      port        = 80
      target_port = 80
    }
  }
}
```

### Check Work
