---
layout: post
title: Parallels Boot2docker Vagrant
lang: ru-RU
tags:
  - parallels
  - Boot2docker
  - Vagrant
  - docker
  - bash
  - profile
  - Troubleshooting
---
Настройка рабочего окружения под управлением boo2docker через parallels.

<!--more-->

[Мой Vagrantfile](https://github.com/iMega/boot2docker)

Написать в ~/.bash_profile

```
export DOCKER_HOST_IP=$(cd ~/Users/irvis/GoogleDrive/projects/tools/boot2docker;vagrant ssh-config | sed -n "s/[ ]*HostName[ ]*//gp")
export DOCKER_HOST="tcp://${DOCKER_HOST_IP}:2375"
```

Добавить в /etc/exports

```
/Users/irvis/GoogleDrive/projects/ -alldirs -mapall=501:20 -network 10.0.3.0 -mask 255.255.255.0
```

Настройка сети parallels

```
$ prlsrvctl net set Shared --ip 10.0.3.2/24 --dhcp-server on --dhcp-ip 10.0.3.1 --ip-scope-start 10.0.3.3 --ip-scope-end 10.0.3.254
```

## Запуск VM
```
$ vagrant up
```
## Выключить VM
```
$ vagrant halt
```
## Удалить VM
```
$vagrant destroy
```
## Решение проблем
`The forwarded port to 2375 is already in use on the host machine.`

```
$ prlsrvctl net list
```
remove rule with port 2375

```
$ prlsrvctl net set Shared --nat-tcp-del tcp2375
```
