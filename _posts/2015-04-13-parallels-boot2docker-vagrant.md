---
layout: post
title: Parallels Boot2docker Vagrant
---

Setup DevEnv to running boo2docker through parallels.

[My Vagrantfile](https://github.com/iMega/boot2docker)

Add your ~/.bash_profile. Your docker client connect to docker server.

```
export DOCKER_HOST_IP=$(cd ~/PATH/TO/DEVENV/tools/boot2docker;vagrant ssh-config | sed -n "s/[ ]*HostName[ ]*//gp")
export DOCKER_HOST="tcp://${DOCKER_HOST_IP}:2375"
```

Add your /etc/exports

```
/PATH/TO/DEVENV/ -alldirs -mapall=501:20 -network 10.0.3.0 -mask 255.255.255.0
```

Set network of parallels

```
$ prlsrvctl net set Shared --ip 10.0.3.2/24 --dhcp-server on --dhcp-ip 10.0.3.1 --ip-scope-start 10.0.3.3 --ip-scope-end 10.0.3.254
```

## Start VM
```
$ vagrant up
```
## Halt VM
```
$ vagrant halt
```
## Destroy VM
```
$vagrant destroy
```
## Troubleshooting
`The forwarded port to 2375 is already in use on the host machine.`

```
$ prlsrvctl net list
```
remove rule with port 2375

```
$ prlsrvctl net set Shared --nat-tcp-del tcp2375
```
