---
title:  "Kubernetes Controllers Leader Election with client-go Library Usage Example"
excerpt: "Using client-go/tools/leaderelection package."

categories:
  - kubernetes
tags:
  - [kubernetes]

classes: wide
toc: true
toc_sticky: true
 
date: 2020-08-04
last_modified_at: 2020-08-04
---
# Kubernetes Controllers Leader Election with client-go Library Usage Example
If you set replicas for HA to your controllers. You need Leader Election.

Kubernetes has a leader election library on the client-go/tools/leaderelection package. This library uses Endpoints created by deployment creation for leader election lock. Elected Leader updates this filed periodically with timestamp. If the leader has problem and the timestamp outdated (current time passed the (renewTime + leaseDurationSeconds), new leader get the lock.

Leader Information is updated into the Endpoint’s annotations fields.

![kubernetes kube-controller-manager endpoints describe](/assets/img/kubernetes/2020-12-23-18-44-04.png)

- "control-plane.alpha.kubernetes.io/leader" key's value has related information.
- "holderIdentity" : leader id
- "acquireTime" : current leader acquired time
- "renewTime" : current leader's leader renew time
- "leaseDurationsSeconds" : leader lease duration time
- "leaderTransitions" : leader trasition count

Kubernetes use this library for kube-controller-manager, kube-scheduler and so on.

## Kube-controller-manager usage example
### File Path
```
cmd/kube-controller-manager/app/controllermanager.go
```

### Git Permalink
- https://github.com/kubernetes/kubernetes/blob/dff82dc0de47299ab66c83c626e08b245ab19037/cmd/kube-controller-manager/app/controllermanager.go#L255

### Packages used
```
"k8s.io/client-go/tools/leaderelection"
"k8s.io/client-go/tools/leaderelection/resourcelock"
```

### Code Example
```
// add a uniquifier so that two processes on the same host don't accidentally both become active
    id = id + "_" + string(uuid.NewUUID())
rl, err := resourcelock.New(c.ComponentConfig.Generic.LeaderElection.ResourceLock,
        c.ComponentConfig.Generic.LeaderElection.ResourceNamespace,
        c.ComponentConfig.Generic.LeaderElection.ResourceName,
        c.LeaderElectionClient.CoreV1(),
        c.LeaderElectionClient.CoordinationV1(),
        resourcelock.ResourceLockConfig{
            Identity:      id,
            EventRecorder: c.EventRecorder,
        })
    if err != nil {
        klog.Fatalf("error creating lock: %v", err)
    }
leaderelection.RunOrDie(context.TODO(), leaderelection.LeaderElectionConfig{
        Lock:          rl,
        LeaseDuration: c.ComponentConfig.Generic.LeaderElection.LeaseDuration.Duration,
        RenewDeadline: c.ComponentConfig.Generic.LeaderElection.RenewDeadline.Duration,
        RetryPeriod:   c.ComponentConfig.Generic.LeaderElection.RetryPeriod.Duration,
        Callbacks: leaderelection.LeaderCallbacks{
            OnStartedLeading: run,
            OnStoppedLeading: func() {
                klog.Fatalf("leaderelection lost")
            },
        },
        WatchDog: electionChecker,
        Name:     "kube-controller-manager",
    })
    panic("unreachable")
```

### Tips
Example code is so clear to understand. These are few tips though.

- add your controller login to run into "leaderelection.LeaderElectionConfig.Callbacks.OnStartedLeading" field.
- 'id' should be unique, if your controllers can run on the same host.
- os.Hostname() can be used for your id. (if you set your controllers can’t run on the same host)
- the 'Name' of leaderelection.LeaderElectionConfig should be the name of your endpoints name created by your deployment.
- WatchDog can be null if you don't use it.