---
layout: post
title: Autostart Jenkins slave on Mac OS X
lang: en-US
tags:
  - jenkins
  - mac-os-x
  - continuous integration
  - automation
---
The com.jenkins.slave.plist in ~/Library/LaunchAgents
<!--more-->

The com.jenkins.slave.plist in ~/Library/LaunchAgents:

```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
        <key>Label</key>
        <string>com.jenkins.slave</string>
        <key>ProgramArguments</key>
        <array>
                <string>/usr/bin/java</string>
                <string>-jar</string>
                <string>/Users/jenkins/slave.jar</string>
                <string>-jnlpUrl</string>
                <string>https://YOUR_JENKINS/computer/mac-os-x/slave-agent.jnlp</string>
                <string>-noCertificateCheck</string>
                <string>-secret</string>
                <string>YOUR_SECRET</string>
        </array>
        <key>RunAtLoad</key>
        <true/>
        <key>KeepAlive</key>
        <true/>
        <key>StandardOutPath</key>
        <string>/Users/jenkins/stdout.log</string>
        <key>StandardErrorPath</key>
        <string>/Users/jenkins/error.log</string>
</dict>
</plist>
```

Change text YOUR_JENKINS and YOUR_SECRET.
