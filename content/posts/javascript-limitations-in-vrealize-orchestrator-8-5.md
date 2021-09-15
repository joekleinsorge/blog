---
title: 'Javascript Limitations in vRealize Orchestrator 8.5'
date: 2021-09-14T21:03:57-04:00
draft: false
feature_image: 'https://images.unsplash.com/photo-1517694712202-14dd9538aa97?crop=entropy&cs=tinysrgb&fit=max&fm=jpg&ixid=MnwxMTc3M3wwfDF8c2VhcmNofDEwfHxqYXZhc2NyaXB0fGVufDB8fHx8MTYyOTc1MDgyNQ&ixlib=rb-1.2.1&q=80&w=2000'
---

One of the selling points for vRealize Orchestrator is the ability to run
JavaScript inside of a workflow using \"Scriptable Tasks\". With the release of
vRealize Automation (vRA) 8, was the addition of [polyglot scripting](https://code.vmware.com/samples/7325/vro-polyglot-scripts). While this is a
great development for the project, I would have much preferred them to implement
modern JavaScript ([ES6 +](https://www.w3schools.com/js/js_es6.asp)) instead.

As of vRealize Orchestrator 8.4.2, VMware is using a modified ES5 engine. Here
are some things that work in this implementation.

Now here are some things that you might expect to work in vRO, but will return
validation errors.
