---
title: React Native project build failed in Xcode 12.5
description: 
category: blog
lang: en
tag: react native
---
My React Native project stopped building after auto update Xcode to 12.5 version.

Build error message:
```
<project>/ios/Pods/Headers/Private/Flipper-Folly/folly/synchronization/DistributedMutex-inl.h:1051:5: 'atomic_notify_one<unsigned long>' is unavailable
```

I found solution in [here](https://github.com/facebook/flipper/issues/2215#issuecomment-827422023).

### Solution:

1. Open `<project>/ios/Pods/Flipper-Folly/folly/synchronization/DistributedMutex-inl.h`
2. Find `atomic_` constructions, and replace to `folly::atomic_`, example: `atomic_notify_one(state)` replace to `folly::atomic_notify_one(state)`
