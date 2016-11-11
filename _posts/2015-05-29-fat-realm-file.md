---
layout: post
title: FAT FAT FAT
lang: en-US
tags:
  - objective-c
  - ios
  - iphone
  - realm
  - database
---
I made app with Realm. A few weeks later, I began to get negative reviews, my app falls. I'm checked and got a following error.

<!--more-->

I made app with Realm. A few weeks later, I began to get negative reviews, my app falls. I'm checked and got a following error.

```
Fatal Exception: RLMException
mmap() failed: Cannot allocate memory
```

File database was 890MB. What makes the app code? Very modestly...

```
for (NSDictionary *data in array) {
    [model createOrUpdateInDefaultRealmWithValue:data];
}
```

I found a similar error on [github](https://github.com/realm/realm-cocoa/issues/1159). But it became easier not, I used ARC (Automatic Reference Counting) in app. The idea occurred to me.

I wrote in AppDelegate. This method should be called before the transaction in Realm.

```
- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions
{
    UIApplication *app = [UIApplication sharedApplication];
    [StorageClient compress];
...
}
```

In StorageClient

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
Now the size of a little more than 300KB.
