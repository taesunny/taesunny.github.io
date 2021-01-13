---
title:  "How to Install Kata Containers and Setting Docker Runtime to Use Kata Containers on CentOS"
excerpt: "About How to Install Kata Containers and Setting Docker Runtime to Use Kata Containers on CentOS"

categories:
  - cloud
tags:
  - [docker, kata, katacontainers, kata-containers, install, runtime]

toc: true
toc_sticky: true
 
date: 2021-01-13
last_modified_at: 2021-01-13
---

# Intro

Kata Containers를 설치하고 Docker의 Runtime에 설정해서 사용 하는 방법을 정리한다.

## 테스트 환경

- CentOS: 7
- Kernel: 3.10.0
- Docker Version: v19.03.9

---
# Install Kata Containers

Host에 Kata Containers를 설치해 준다.

## Install Kata Containers pacakges

[[설치 문서](https://github.com/kata-containers/documentation/blob/master/install/centos-installation-guide.md)]를 참고해 관련 패키지들을 설치해 준다.

> VERSION 변수를 [[설치 문서 링크](https://github.com/kata-containers/documentation/blob/master/install/centos-installation-guide.md)]를 확인해 설정한다. ex) CentOS 7 인 경우 VERSION=7

> yum-config-manager —add-repo 명령으로 repo 추가 시, 설치 문서 가이드를 이용해 repo url 구성이 힘들 경우 [[레포지터리 사이트](http://download.opensuse.org/repositories/home:/katacontainers:/releases:/x86_64:/)]에서 url을 확인 할 수 있다.
> 
> ex) CentOS7, v1.11: http://download.opensuse.org/repositories/home:/katacontainers:/releases:/x86_64:/stable-1.11/CentOS_7/home:katacontainers:releases:x86_64:stable-1.11.repo

```bash
$ source /etc/os-release
$ sudo yum -y install yum-utils
$ ARCH=$(arch)
$ BRANCH="${BRANCH:-master}"
$ VERSION="{your version}"
$ sudo -E yum-config-manager --add-repo "http://download.opensuse.org/repositories/home:/katacontainers:/releases:/${ARCH}:/${BRANCH}/CentOS_${VERSION_ID}/home:katacontainers:releases:${ARCH}:${BRANCH}.repo"
$ sudo -E yum -y install kata-runtime kata-proxy kata-shim
```

- 패키지 설치 확인

    kata-proxy, kata-runtime, kata-shim 패키지와 의존성 패키지들이 설치 된 것을 확인 할 수 있다.

    > 기본으로 qemu-vanilla 버전이 설치된다. 필요에 따라 다른 VMM들을 설치 및 설정할 수 있다.

    ![kata containers installed packages](/assets/img/kubernetes/2021-01-12-21-01-28.png)

## Kata Containers Host Check

kata-runtime의 kata-check 명령으로 Kata Containers를 사용할 수 있는지를 테스트 할 수 있다.

```bash
sudo kata-runtime kata-check
```

- 결과 예시

    ![kata-runtime kata-check](/assets/img/kubernetes/2021-01-12-21-01-58.png)

---

# Install Docker
yum을 이용해 Docker를 설치해 준다.

## Set up the repository
쉘에서 다음 명령어들을 실행하여 repo를 설치해 준다.

```
$ sudo yum install -y yum-utils

$ sudo yum-config-manager \
    --add-repo \
    https://download.docker.com/linux/centos/docker-ce.repo
```

## Install Docker Engine
쉘에서 다음 명령어를 사용해 docker를 설치해 준다.

```
$ sudo yum install docker-ce docker-ce-cli containerd.io
```

## Start Docker
Docker 데몬을 실행해 준다.

```
sudo systemctl start docker
```

---
# Setting Docker runtime to use Kata Containers

방법 1 또는 방법 2를 사용해 Docker의 runtime을 kata-runtime으로 설정한다.

## 방법1 - systemd를 사용한 방법
Docker의 systemd 설정에 kata-container를 runtime으로 추가 한 후, 필요에 따라 default runtime으로 설정하는 인자를 추가한다.

> runtime default 설정: 'runc'

```
$ sudo mkdir -p /etc/systemd/system/docker.service.d/
$ cat <<EOF | sudo tee /etc/systemd/system/docker.service.d/kata-containers.conf
[Service]
ExecStart=
ExecStart=/usr/bin/dockerd -D --add-runtime kata-runtime=/usr/bin/kata-runtime --default-runtime=kata-runtime
EOF
```

## 방법2 - daemon.json 파일 수정을 사용한 방법
Docker의 설정파일인 daemon.json 파일에 kata-container를 runtime으로 추가 한 후, 필요에 따라 default runtime으로 설정하는 인자를 추가한다.

> runtime default 설정: 'runc'

- 디렉터리가 없을 경우 생성
    ```
    $ sudo mkdir -p /etc/docker
    ```

- '/etc/docker/daemon.json' 파일을 추가 하고, 아래의 내용을 추가한다.
    ```
    {
      "default-runtime": "kata-runtime",
      "runtimes": {
        "kata-runtime": {
          "path": "/usr/bin/kata-runtime"
        }
      }
    }
    ```
  
## Docker 데몬 재 시작
Docker 데몬을 재 시작하여 설정을 적용해 준다.

```
$ sudo systemctl daemon-reload
$ sudo systemctl restart docker
```

---
# Kata Containers Verification with Docker
Docker를 생성 하여 Kata Containers가 Docker의 runtime으로 잘 설정 되었는지를 확인한다.

- Host Kernel 확인

```
uname -a
```

![Host Kernel Check](/assets/img/cloud/2021-01-13-19-12-08.png)


- Kata Containers를 사용한 Docker Container Kernel 확인
    - Host Kernel과 Kata Containers 내부의 Kernel이 다른 것을 확인 할 수 있다.

```
docker run --runtime=kata-runtime busybox uname -a
```

![Kata Containers Kernel Check](/assets/img/cloud/2021-01-13-19-10-18.png)

- runc를 사용한 Docker Container Kernel 확인
    - Host Kernel과 Container 내부의 Kernel이 동일한 것을 확인 할 수 있다.

```
docker run --runtime=runc busybox uname -a
```

![Runc Container Kernel Check](/assets/img/cloud/2021-01-13-19-11-17.png)


---
# Source

- [https://github.com/kata-containers/documentation/blob/master/Developer-Guide.md](https://github.com/kata-containers/documentation/blob/master/Developer-Guide.md)
- [https://github.com/kata-containers/documentation/blob/master/install/centos-installation-guide.md](https://github.com/kata-containers/documentation/blob/master/install/centos-installation-guide.md)
- [https://github.com/kata-containers/documentation/blob/master/install/docker/centos-docker-install.md](https://github.com/kata-containers/documentation/blob/master/install/docker/centos-docker-install.md)
- [https://docs.docker.com/engine/install/centos/](https://docs.docker.com/engine/install/centos/)