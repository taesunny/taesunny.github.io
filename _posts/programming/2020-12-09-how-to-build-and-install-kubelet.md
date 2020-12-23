---
title:  "How to build and install Kubernetes Kubelet for your customized Kubelet"
excerpt: "I tested to build and install Kubelet using Kubernetes project."

categories:
  - programming
tags:
  - [programming]

toc: true
toc_sticky: true

comments: true
 
date: 2020-12-09
last_modified_at: 2020-12-09
---
Kubelet doesn’t provide all the features we want. For example, setting cgroup or oom kill policy.

So, I tested to build and install Kubelet using Kubernetes project.

# Prepare build environment
First, Go has to be installed. Some K8S Version matches old Go version. I tested to build v1.16.3 K8S Project, and i should use the go v1.12.15 version. The latest project has no problem with the latest go version (tested with v1.15.6).
- related git issue: https://github.com/kubernetes/kubernetes/issues/78845

Second, some packages should be installed. I installed gcc package. I failed to find the exact list yet. So, check the list on the error message.

# How to Build Kubelet
Kubernetes project has a Makefile on the project root path, it has a general build feature with go and a bazel build feature.

You can use ‘make all’ and set a target using ‘WHAT=’ argument like below.

```
make all WHAT=cmd/kubelet GOFLAGS=-v
```

The build guide using Makefile (/build/root/Makefile) capture below.

![make build guide capture](/assets/img/kubernetes/2020-12-19-00-28-31.png)

And the ‘make all’ command uses the ‘/hack/make-rules/build.sh’ script.

![/hack/make-rules/build.sh capture
](/assets/img/kubernetes/2020-12-19-00-32-00.png)

It builds a binary for the system version where the make is running in default setting. And it places this binary to the path ‘/{k8s project dir}/_output/bin’.

# How to Install
In general, we install Kubelet using yum or apt.

Let’s check the rpm spec file from k8s v1.17 version.

> The k8s project’s ‘/build/rpms/’ directory has been deleted from v1.18 version in the commit. I will check the latest kubelet.spec file and update this information soon, if possible.

![kubelet rpm spec file](/assets/img/kubernetes/2020-12-19-00-34-08.png)

It copies the Kubelet binary to the system (like ‘/usr/bin’). And it adds systemd service file (image below) to the systemd system path (like ‘/usr/lib/systemd/system/’ or ‘/etc/systemd/system/’.

![kubelet systemd service file](/assets/img/kubernetes/2020-12-19-00-34-54.png)

You can rpm-build your binary using the spec file and install Kubelet using apt or yum.

# Installation Test
You can easily test your binary on the running k8s environment.

Follow the steps

1. Access your node for Kubelet test
2. Stop the Kubelet
    ```
    sudo systemctl stop kubelet
    ```
3. Install your binary
    > Backup the original binary before run the command below
    ```
    sudo mv ./kubelet /usr/bin
    ```
4. Check the new Kubelet version
    ```
    kubelet --version
    ```
5. Start the Kubelet
    ```
    sudo systemctl start kubelet
    ```
6. Check the node status and Kubelet version
    ```
    kubectl get nodes
    ```
    ![nodes info with the new installed kubelet](/assets/img/kubernetes/2020-12-19-00-40-00.png)

    You can check the Ready status node and the version same with the Kubelet version from the step 3.

# Build Test Environment/Kubernetes Version
- Ubuntu 18.04
- Kubernetes v1.16.3, v1.17.9, v1.18.0 and v1.19.0

# Kubelet Connection Test Environment/Kubernetes Version
- Ubuntu 18.04
- Kubernetes v1.16.3

# Source
- https://github.com/kubernetes/kubernetes