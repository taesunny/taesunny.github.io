---
title:  "[Test] Test if cgroup pids.max value limits both of processes and threads number"
excerpt: ""

categories:
  - etc
tags:
  - [etc, test, pid, cgroup]

toc: true
toc_sticky: true
 
date: 2020-11-10
last_modified_at: 2020-11-10
---

# Intro

Test if cgroup pids.max value limits both of processes and threads number using docker with 'pids-limit' config.

---

# Test

## Test Environment

- CentOS 7
- Kernel Linux 3.10.0
- Cgroup v1


## Test Program

Using a python program creating processes or threads with the number user entered.

- Process mode or thread mode can be selected by set ‘MODE’ environment.
- The number of processes or threads to create can be set by ‘HOWMANY’ environment.
- You can check the README.md file of the git project.
- You can also use the docker image from dockerhub or you can build it yourself using Dockerfile

> [Python Program Github repo](https://github.com/taesunny/cgroup-pid-max-test)

> [Docker Image](https://hub.docker.com/r/taesunny/cgroup-pid-max-test)


## Create Docker with the ‘ pids-limit’ config and envs used in the test program

For a test, i set cgroup pid limit value to ‘100’ and i tried to create 110 threads.

```
// example
docker run -i -t -e MODE=thread -e HOWMANY=110 --pids-limit 100 taesunny/cgroupo-pid-max-test:1.2
```

{:refdef: style="text-align: center;"}
![test result](/assets/img/etc/2021-01-27-19-00-52.png)
{: refdef}

## Result

The program can’t create threads more than cgroup pids limit.

> Pid cgroup’s pids.max value limits both of processes and threads number.

- In the test, Cgroup PID Limit value limits also thread numbers.

{:refdef: style="text-align: center;"}
![test result](/assets/img/etc/2021-01-27-19-02-57.png)
{: refdef}
