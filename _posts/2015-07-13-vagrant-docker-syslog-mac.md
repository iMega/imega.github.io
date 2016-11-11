---
layout: post
title: Vagrant Boot2Docker Syslog
lang: en-US
tags:
  - parallels
  - Boot2docker
  - Vagrant
  - docker
  - bash
  - mac-os-x
  - Syslog
---
A short story about how to make work for yourself Syslog.

<!--more-->

## Start syslogd on mac

Convert plist to XML

```
$ sudo plutil -convert xml1 /System/Library/LaunchDaemons/com.apple.syslogd.plist
$ sudo vi /System/Library/LaunchDaemons/com.apple.syslogd.plist
```
Insert in dict of key "Sockets"

```
<key>Sockets</key>
<dict>
    ...
    <key>NetworkListener</key>
    <dict>
        <key>SockServiceName</key>
        <string>syslog</string>
        <key>SockType</key>
        <string>dgram</string>
    </dict>
</dict>
```
Conver xml to binary

```
$ sudo plutil -convert binary1 /System/Library/LaunchDaemons/com.apple.syslogd.plist
```
And reload syslogd

```
sudo launchctl unload /System/Library/LaunchDaemons/com.apple.syslogd.plist
sudo launchctl load /System/Library/LaunchDaemons/com.apple.syslogd.plist
```

## OR

```
$ cd /System/Library/LaunchDaemons
$ sudo /usr/libexec/PlistBuddy -c "add :Sockets:NetworkListener dict" com.apple.syslogd.plist
$ sudo /usr/libexec/PlistBuddy -c "add :Sockets:NetworkListener:SockServiceName string syslog" com.apple.syslogd.plist
$ sudo /usr/libexec/PlistBuddy -c "add :Sockets:NetworkListener:SockType string dgram" com.apple.syslogd.plist
$ sudo launchctl unload /System/Library/LaunchDaemons/com.apple.syslogd.plist
$ sudo launchctl load /System/Library/LaunchDaemons/com.apple.syslogd.plist
```
Thanks [Gordon Davisson](http://stackexchange.com/users/32384/gordon-davisson)

## Check syslog

```
$ netstat -an | grep udp | grep 514
```

## Test remote syslogd
```
$ syslogd -R YOUR_IP
$ logger hello: bla-bla-bla
```

## NFS on host
```
/Users/irvis/projects/ -alldirs -mapall=501:20 -network 10.0.3.0 -mask 255.255.255.0
```

## Vagrantfile
~/projects/imega/boot2docker/Vagrantfile

```
VAGRANTFILE_API_VERSION = "2"

Vagrant.require_version ">= 1.7.0"

PROJECT_ROOT = File.expand_path(Dir.pwd + "/../..")

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.vm.box = "parallels/boot2docker"
  config.vm.synced_folder PROJECT_ROOT, PROJECT_ROOT, create: true, type: "nfs", mount_options: ["nolock", "vers=3", "udp"], id: "nfs-sync", nfs_export: false
  config.vm.provision "shell", inline: "sudo cp -rf /var/lib/boot2docker/tls " + PROJECT_ROOT + "/"
  config.vm.provision "shell", inline: "syslogd -R 10.0.3.2"
end
```
10.0.3.2 is ip-addr my host in network of parallels.

## Profile
```
export DOCKER_HOST_IP=$(cd ~/projects/imega/boot2docker;vagrant ssh-config | sed -n "s/[ ]*HostName[ ]*//gp")
export DOCKER_TLS_VERIFY="1"
export DOCKER_CERT_PATH="~/projects/tls"
export DOCKER_HOST="tcp://${DOCKER_HOST_IP}:2376"
```
