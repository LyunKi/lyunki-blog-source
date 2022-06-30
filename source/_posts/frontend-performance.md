---
title: 前端性能量化
---

工作过程中，经常需要表现下自己的工作成果，但是所谓空口无凭，数据才是最直观的，刚好，贴心的浏览器提供了 performance API <!-- more -->

## performance.memory

能够通过 performance.memory 获得以下数据 （都以字节计算

1. `usedJSHeapSize` : 当前 JS 堆活跃段（segment）的体积 The currently active segment of JS heap, in bytes.
2. `totalJSHeapSize` : 已分配的堆体积 The total allocated heap size, in bytes.
3. `jsHeapSizeLimit` : 上下文内可用堆的最大体积 The maximum size of the heap, in bytes, that is available to the context
