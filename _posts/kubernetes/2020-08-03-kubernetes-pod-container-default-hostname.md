---
title:  "Kubernetes Pod Container Default hostname"
excerpt: ""

categories:
  - kubernetes
tags:
  - [kubernetes, pod, container, hostname]

toc: true
toc_sticky: true
 
date: 2020-08-03
last_modified_at: 2020-08-03
---

## Conclusion
Container’s default hostname is set to pod’s name.

> If you use ‘hostNetwork’ option, Container’s hostname is set to the physical host’s name where container is running.

## Test

- Create a basic ubuntu pod

{:refdef: style="text-align: center;"}
![k8s ubuntu pod yaml example](/assets/img/kubernetes/2021-03-12-10-54-53.png)
{: refdef}

- Exec the pod container’s sh and check hostname

{:refdef: style="text-align: center;"}
![k8s exec container sh and check](/assets/img/kubernetes/2021-03-12-10-55-22.png)
{: refdef}