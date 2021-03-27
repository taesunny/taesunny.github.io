---
title:  "Helm - Kubernetes Application Managing Tool"
excerpt: ""

categories:
  - kubernetes
tags:
  - [kubernetes, helm]

toc: true
toc_sticky: true
 
date: 2021-03-27
last_modified_at: 2021-03-27
---

# Intro

Helm은 Kubernetes Application Managing Tool이다. Helm을 통해 Kubernetes 상의 Application들을 관리할 수 있다. CNCF의 Graduated Project로 Helm Community에 의해 관리된다.

---

# 개념

## Chart

Chart는 Helm에서 관리 할 package 단위이다. Chart는 Package와 관련된 Kubernetes 상의 자원들을 정의한다. Chart를 이용해 HTTP Server, Database 와 같은 Application들을 kubernetes 상에 설치한다.

---

# Usage

## Helm 설치

Ubuntu 에서는 snap 패키지 관리 툴을 지원한다. snap을 이용해 Helm을 설치 가능하다. 아래의 snap 명령을 이용해 helm을 설치한다. (OS별 설치 방법 혹은 스크립트를 통한 설치 방법은 [공식 가이드](https://helm.sh/ko/docs/intro/install/)를 참고)

- 설치 명령:

```bash
$ sudo snap install helm --classic
```

- 설치 확인 명령:

```bash
$ helm version
```

- helm 버전 확인

{:refdef: style="text-align: center;"}
![helm version check](/assets/img/kubernetes/2021-03-27-15-19-48.png)
{: refdef}

## Helm Chart Repository 추가

Helm에서 사용할 Chart Repository를 추가 할 수 있다. Helm 설치 직후에는 repository 목록에 등록된 Repository가 없으므로 아래의 명령을 실행하여 공식 Helm stable Repository를 추가한다.

- 공식 Helm stable Repository 추가 명령:

```bash
$ helm repo add stable https://charts.helm.sh/stable
```

- Repository 추가 명령 실행 및 확인:

{:refdef: style="text-align: center;"}
![helm repo add](/assets/img/kubernetes/2021-03-27-15-20-31.png)
{: refdef}

## 기타 명령

### Chart 검색

아래의 helm search 명령을 통해 repository 내의 chart들을 검색 할 수 있다.

```bash
$ helm search repo <chart repo/chart name>
$ helm search repo <chart repo>

// example
$ helm search repo stable/sysdig
```

### Chart 목록 업데이트

helm repo update 명령을 통해 repository들의 chart 목록을 최신화 할 수 있다.

```bash
$ helm repo update
```

### Chart Pull

chart 설치 없이 chart를 다운로드하거나 확인하기 위해서는 helm pull 명령을 사용한다.

```bash
$ helm pull <chart repo/chart name>
```

### Chart 설치

helm install 명령을 통해 chart를 설치할 수 있다.

```bash
$ helm install <release name> <chart repo/chart name>
$ helm install <chart repo/chart name> --generate-name
```

- 예시:

{:refdef: style="text-align: center;"}
![helm install](/assets/img/kubernetes/2021-03-27-15-21-18.png)
{: refdef}

### Chart 커스터마이징

chart 설치 시 옵션들을 확인하고, 수정할 수 있다.

helm show values 명령을 통해 가능한 옵션들을 확인한다.

```bash
$ helm show values <chart repo/chart name>
```

- 예시:

{:refdef: style="text-align: center;"}
![helm show](/assets/img/kubernetes/2021-03-27-15-22-11.png)
{: refdef}

각 설정들을 오버라이드하여, chart를 설치할 수 있다.

- file을 통한 오버라이드 예시:

```bash
$ echo '{nameOverride: user-sunny}' > config.yaml
$ helm install -f config.yaml stable/wordpress --generate-name
```

- 명령줄을 통한 오버라이드 예시:

```bash
$ helm install stable/wordpress --generate-name --set nameOverride=user-sunny
```

### 설치된 Chart 목록 확인

helm ls 명령을 통해 설치된 chart들을 확인 할 수 있다.

```bash
$ helm ls
```

- 예시:

{:refdef: style="text-align: center;"}
![helm ls](/assets/img/kubernetes/2021-03-27-15-22-46.png)
{: refdef}

### 설치된 Chart 삭제

helm uninstall 혹은 helm delete 명령을 통해 설치된 chart들을 삭제할 수 있다.

```bash
$ helm uninstall <chart name>
$ helm delete<chart name>
```

- 예시:

{:refdef: style="text-align: center;"}
![helm uninstall](/assets/img/kubernetes/2021-03-27-15-23-12.png)
{: refdef}

### Chart 정보 확인

helm show 명령을 통해 chart의 정보를 확인 할 수 있다.

```bash
$ helm show chart <chart repo/chart name>
```

- 예시:

{:refdef: style="text-align: center;"}
![helm show](/assets/img/kubernetes/2021-03-27-15-24-32.png)
{: refdef}

### Chart 업그레이드

helm upgrade 명령을 통해 설치된 chart의 release를 업그레이드 할 수 있다. -f, --set 플래그를 통해 옵션 수정도 가능하다.

```bash
$ helm upgrade [RELEASE version] [CHART name] [flags]
```

---

# Source

- [https://helm.sh/ko/docs](https://helm.sh/ko/docs/intro/quickstart/)
- [https://helm.sh/ko/docs/topics/charts/](https://helm.sh/ko/docs/topics/charts/)
- [https://helm.sh/](https://helm.sh/)