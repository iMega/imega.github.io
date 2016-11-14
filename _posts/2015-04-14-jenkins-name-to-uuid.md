---
layout: post
title: How to jenkins use the username for the repository UUID instead
lang: en-US
tags:
  - jenkins
  - job-dsl
  - Credential
  - continuous integration
  - Troubleshooting
  - automation
---
Using JOB-DSL-plugin has a little trouble when you use scm section
<!--more-->

Using [JOB-DSL-plugin](https://github.com/jenkinsci/job-dsl-plugin/wiki) has a little trouble when you use scm section

```
scm {
    git {
        remote {
            name('origin')
                url(repo)
                credentials(myCredential)
        }
        branch('master')
    }
}
```
You must write UUID in variable "MyCredential". Who can remember?

<!--more-->

The decision turned out to be fairly simple:

```
import jenkins.model.Jenkins;
import com.cloudbees.plugins.credentials.CredentialsProvider;
import com.cloudbees.plugins.credentials.common.StandardCredentials;
import hudson.Plugin;
import static hudson.security.ACL.SYSTEM;

Jenkins jenkins = Jenkins.getInstance();
Plugin credentialsPlugin = jenkins.getPlugin("credentials");
```
This is part of the config, which specifies the username. You see a list of users in the jenkins.

```
def userDescription = 'UserName';
def repo = 'git@github.com:iMega/imega.github.io.git';
```
You run loop over the list and pick the correct UUID

```
if (credentialsPlugin != null) {
    for (CredentialsProvider credentialsProvider : jenkins.getExtensionList(CredentialsProvider.class)) {
        for (StandardCredentials credentials : credentialsProvider.getCredentials(StandardCredentials.class, jenkins, SYSTEM)) {
            if (credentials.getDescription().equals(userDescription)) {
                myCredential = credentials.getId();
            }
        }
    }
}
```

Example job:

```
job {
    name 'job_init'
    logRotator(-1, 4, -1, -1)
    scm {
        git {
            remote {
                name('origin')
                url(repo)
                credentials(myCredential)
            }
            branch('master')
        }
    }
}
```
Good luck in the automation!
