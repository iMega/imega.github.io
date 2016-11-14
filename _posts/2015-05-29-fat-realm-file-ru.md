---
layout: post
title: Жирный Жирный Жирный...
lang: ru-RU
tags:
  - objective-c
  - ios
  - iphone
  - realm
  - database
---
Делал приложеньку и выбор компонента в качестве базюки пал на Realm. Спустя несколько недель, стал получать негативные отзывы  от людей которые стали использовать приложение - "Вылетает!" кричали раздраженно.
<!--more-->

Делал приложеньку и выбор компонента в качестве базюки пал на Realm. Спустя несколько недель, стал получать негативные отзывы  от людей которые стали использовать приложение - "Вылетает!" кричали раздраженно.

По детальному описанию, как это воспроизвести стал проверять и наткнулся на следующую ошибку.

```
Fatal Exception: RLMException
mmap() failed: Cannot allocate memory
```

Файл базы данных сказать большой это мало, при данных порядка в пару метров, размер был 890MB, а в коде была совершенно безобидная строка:

```
for (NSDictionary *data in array) {
    [model createOrUpdateInDefaultRealmWithValue:data];
}
```

По гуглив нашел "единомышленников" на [гитхабе](https://github.com/realm/realm-cocoa/issues/1159). Предложение не плохое, но требовало работы приложения без использования ARC.

После нескольких часов попыток хоть как то исправить ситуацию...

Вот решение для приложения с ARC. В AppDelegate добавим метод, самое главное он должен быть перед транзакцией с Realm.

```
- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions
{
    UIApplication *app = [UIApplication sharedApplication];
    [StorageClient compress];
...
}
```

Мой StorageClient

```
+ (void)compress
{
    NSString *defaultPath = RLMRealm.defaultRealmPath;
    NSString *compressPath = [[defaultRealmPath stringByDeletingLastPathComponent] stringByAppendingPathComponent:@"old.realm"];
    NSError *error = nil;
    [[NSFileManager defaultManager] moveItemAtPath:defaultPath toPath:compressPath error:&error];

    RLMRealm *copy = [RLMRealm realmWithPath:compressPath];
    [copy writeCopyToPath:defaultPath error:nil];
    [[NSFileManager defaultManager] removeItemAtPath:compressPath error:&error];
    copy = nil;
}
```

Теперь размер файла стал немного более 300KB.
