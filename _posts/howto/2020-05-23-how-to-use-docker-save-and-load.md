---
title:  "How to save docker image to tar and load from it"
excerpt: "docker save and docker load usage"

categories:
  - howto
tags:
  - [howto, docker, image, tar, save, load]

toc: true
toc_sticky: true
 
date: 2020-05-23
last_modified_at: 2020-05-23
---

---

# Saving a docker image to a tar file

> -o option: set file name

```bash
docker save [option] [image name]:[tag]
```

---

# Loading a tar docker image

> Loaded image name is from the docker saved image name

```bash
docker load -i [tar file name]
```

---

# Source
- https://docs.docker.com/engine/reference/commandline/save/
- https://docs.docker.com/engine/reference/commandline/load/