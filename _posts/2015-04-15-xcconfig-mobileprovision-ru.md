---
layout: post
title: xcconfig mobileprovision
lang: ru-RU
tags:
  - objective-c
  - mobileprovision
  - chell
  - xcconfig
  - ios
---
Есть прекрасная приложенька mobileprovisionParser. С ее помощью можно автоматизировать сборку ios app и буквально на лету переключаться между сборками (я от лени собираю все разом).
<!--more-->

Есть прекрасная приложенька [mobileprovisionParser](https://github.com/0xc010d/mobileprovision-read).

С ее помощью можно автоматизировать сборку ios app и буквально на лету переключаться между сборками (я от лени собираю все разом).

Как я использую:

```
MPP=$WORKSPACE/bin/mobileprovisionParser
chmod +x $MPP
echo Divices
$MPP -f *.mobileprovision -o devices
MP_UUID=$($MPP -f *.mobileprovision -o uuid)

echo "PROVISIONING_PROFILE = $MP_UUID" >> $WORKSPACE/conf/adhoc.xcconfig

cp adhoc.mobileprovision ~/Library/MobileDevice/Provisioning\ Profiles/$MP_UUID.mobileprovision
```

Спасибо Eugene Solodovnykov за клевую штуку.
