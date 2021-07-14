---
title: Центрирование подписей в react-native-charts-wrapper для group BarChart
description:
category: blog
lang: ru
tag: react-native-charts-wrapper
---

Для использования группировки в `<BarChart/>` в свойстве `data` используется опция вида:

```
config: {
  barWidth: 0.2,
  group: {
    fromX: 0,
    groupSpace: 0.1,
    barSpace: 0.1,
  },
}
```

Чтобы подписи под группированными колонками были по центру, нужно рассчитать параметры выше исходя из формулы:

```
(barSpace + barWidth) * 'количество столбцов в группировке' + groupSpace = 1
```
