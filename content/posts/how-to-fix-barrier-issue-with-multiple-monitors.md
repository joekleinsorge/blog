---
title: 'How to Fix Barrier Issue With Multiple Monitors'
date: 2022-01-10T16:44:49-05:00
draft: false
toc: True
cover:
tags:
  - Barrier
  - How-To
---

## What's the issue?

Help me, my mouse got stuck in a corner and I can't get it out!

## What caused the issue?

Barrier doesn't handle relative mapping of multiple displays very well and can create mouse traps, where your mouse will get stuck and never come out of.

My setup looked like this:
{{< image src="/img/barrierBroken.JPG" alt="Barrier Broken Setup Screenshot" style="border-radius: 8px;" >}}

## The Solution

Change the resolution of your monitors to match each other. Ta Duh!

{{< image src="/img/barrierFix.JPG" alt="Barrier Broken Fixed Screenshot" style="border-radius: 8px;" >}}

Now that monitors 1 and 3 are the same resolution, no more trapped mouse!
