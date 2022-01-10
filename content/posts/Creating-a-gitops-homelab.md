---
title: 'Creating a Gitops Homelab'
date: 2021-11-26T18:37:38-05:00
draft: true
toc: true
cover:
tags:
  - gitops
  - homelab
---

## Goal

"GitOps" is the "new" idea to chase in operations. The idea of having one source controlled source of truth for everything is appealing to anyone who has worked in IT ops for a variaty of reasons.

## Base Infra

I used multipass to build a Ubuntu 20.x vm on Hyper-V on me Windows 10 Pro desktop.

```powershell
multipass launch -n k3s
```

However/ wherever you decide to run the ubuntu shouldn't really matter.
