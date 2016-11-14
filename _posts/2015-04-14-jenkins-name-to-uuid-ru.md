---
layout: post
title: Как в jenkins использовать имя пользователя для репозитария вместо UUID
lang: ru-RU
tags:
  - jenkins
  - job-dsl
  - Credential
  - continuous integration
  - Troubleshooting
  - automation
---
Используя [JOB-DSL-plugin](https://github.com/jenkinsci/job-dsl-plugin/wiki) есть маленькая неприятность, когда используешь scm например
<!--more-->

Используя JOB-DSL-plugin есть маленькая неприятность, когда используешь scm например

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

в myCredential приходится указывать не имя, а UUID, фича ли это не знаю, но мне лично не удобно этим оперировать.

<!--more-->

Решение оказалось довольно простым:

```
import jenkins.model.Jenkins;
import com.cloudbees.plugins.credentials.CredentialsProvider;
import com.cloudbees.plugins.credentials.common.StandardCredentials;
import hudson.Plugin;
import static hudson.security.ACL.SYSTEM;

Jenkins jenkins = Jenkins.getInstance();
Plugin credentialsPlugin = jenkins.getPlugin("credentials");
```

Это часть конфига, где указываешь имя пользователя см. список юзеров в jenkins

```
def userDescription = 'UserName';
def repo = 'git@github.com:iMega/imega.github.io.git';
```

Перебираем список и забираем нужный UUID

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

Пример задания:

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
Удачи в автоматизации!
