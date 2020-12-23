---
title:  "Kubernetes GET API Watch Feature"
excerpt: "How to get object update events from API Server."

categories:
  - kubernetes
tags:
  - [analysis, kubelet, kubernetes]

toc: true
toc_sticky: true
 
date: 2020-11-23
last_modified_at: 2020-11-23
---
# Kubernetes GET API Watch Feature

## Purpose
1. How to use GET API with ‘watch’ feature(option).
2. How Watch feature is used by Reflector of Informer.

## How to call APIs of API server in the pod with curl for test.
You can create a pod having roles (creating, deleting, updating certain resources) using Role, RoleBinding and ServiceAccount.

Then your pod has a ca.crt file and a token to call APIs of API server.

Check the path below. Or you can get the path by describe your pod.
```
/var/run/secrets/kubernetes.io/serviceaccount/
```

### Curl Usage
You can curl to api server using the token and cacert file like below
```
curl -i -s --header "Authorization: Bearer $TOKEN" --cacert /var/run/secrets/kubernetes.io/serviceaccount/ca.crt https://{ip address}:{port}/api/v1/namespaces/default/pods
```

## Watch option on GET API
Kubernetes has a watch option when call GET HTTP request.

### Basic Usage
```
GET /api/v1/namespaces/default/pods?watch=1
```
with the ‘watch=1’ query parameter, API Server will give us the resources change information continuously, using HTTP 2.0 streaming feature.

First response will be like below. with “type” : “ADDED”.

![type is added](/assets/img/kubernetes/2020-12-23-18-15-17.png)

And If the resource is updated, another response will be arrived.

Like below. with “type” : “MODIFIED”.

![type is modified](/assets/img/kubernetes/2020-12-23-18-16-15.png)

### EventTypes
Event Type can be
- ADDED // when the object is created or on the first response
- MODIFIED // when the object is updated
- DELETED // when the object is deleted

### ResourceVersion option usage
You can also use resourceVersion query parameter when using watch feature.

Before you use the option, you can get resourceVersion using get request like below.

```
curl -i -s --header "Authorization: Bearer $TOKEN" --cacert /var/run/secrets/kubernetes.io/serviceaccount/ca.crt https://{ip address}:6443/api/v1/namespaces/default/pods
```

![resourceVersion of response](/assets/img/kubernetes/2020-12-23-18-17-51.png)

You can get a resourceVersion using both of List Get API and Single Element Get API.

Then try to use resourceVersion parameter when use watch feature like below.

```
curl -i -s --header "Authorization: Bearer $TOKEN" --cacert /var/run/secrets/kubernetes.io/serviceaccount/ca.crt https://{ip address}:{port}/api/v1/namespaces/default/pods&watch=1&resourceVersion=17128044
```

If you add the parameter, then API server will response the resource data continuously after the resourceVersion, if it is possible.

## WatchFunc of Informer
You can find the watchFunc in the listwatch.go file on the client-go library, and you use it when create ListWatch function for your informer on your controller(operator).

In this case, the reflector of the informer also use the watch feature.

By setting ‘Watch’ value to true, the watch query parameter is set. And a streamWatcher will receive the resource’s modification info responses continuously.

![watchFunc of client-go lib](/assets/img/kubernetes/2020-12-23-18-18-58.png)

The watchFunc will be run in the ListAndWatch function of reflector’s Run() function when informer’s Run() function is called.

![reflector.go file — ListAndWatch](/assets/img/kubernetes/2020-12-23-18-19-27.png)

So, the informer(reflector) can know the resource change information, and it can handle it.

# Source
- https://kubernetes.io/docs/reference/using-api/api-concepts/#efficient-detection-of-changes
- https://github.com/kubernetes/kubernetes