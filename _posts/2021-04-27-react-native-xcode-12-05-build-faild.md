---
title: Ошибка сборки React Native проекта на Xcode 12.5
description: 
category: blog
lang: ru
tag: react native
---
После перезапуска Xcode, перестал собираться проект на React Native. Оказалось, что проблема из-за автоматического 
обновления Xcode до 12.5 версии.

Ошибка при попытке сборки:
```
<project>/ios/Pods/Headers/Private/Flipper-Folly/folly/synchronization/DistributedMutex-inl.h:1051:5: 'atomic_notify_one<unsigned long>' is unavailable
```

Решение нашел вот [тут](https://github.com/facebook/flipper/issues/2215#issuecomment-827422023).

### Решение:

1. Открываем файл `<project>/ios/Pods/Flipper-Folly/folly/synchronization/DistributedMutex-inl.h`
2. Ищем конструкции подобного вида `atomic_`, и меняем на `folly::atomic_`, пример: `atomic_notify_one(state)` меняем на `folly::atomic_notify_one(state)`
