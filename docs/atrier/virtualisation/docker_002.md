---
title: Install Docker
description: 
published: true
date: 2023-04-21T11:17:56.792Z
tags: 
editor: markdown
dateCreated: 2023-04-21T11:17:52.804Z
---

# Install docker

```
sudo dnf install dnf-plugins-core
```

```
sudo dnf install epel-release
```

```
sudo dnf config-manager --set-enabled powertools
```

```
sudo dnf update
```

```
sudo dnf update
```

```
dnf install sudo passwd procps-ng bash-completion
```

```
sudo dnf config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
sudo dnf install docker-ce docker-ce-cli containerd.io
```

```
useradd student
passwd student
```

```
usermod -aG wheel student
```

```
su - student
```

```
sudo systemctl enable docker
```