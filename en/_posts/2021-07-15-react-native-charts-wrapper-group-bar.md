---
title: Centering labels in react-native-charts-wrapper for group BarChart
description:
category: blog
lang: en
tag: react-native-charts-wrapper
---

To use grouping in `<BarChart/>` in the property `data` an option like:

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

The labels under the grouped columns to be centered, you need to calculate the parameters above based on the formula:

```
(barSpace + barWidth) * countInGroup + groupSpace = 1
```
