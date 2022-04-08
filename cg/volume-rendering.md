---
description: direct visualization of volumetric data
---

# Volume Rendering

Direct Volume Rendering (DVR) using Volumetric Path Tracing (VPT) is a scientific visualization technique that simulates light transport with objects’ matter using physically-based lighting models. Monte Carlo (MC) path tracing is often used with surface models, yet its application for volumetric models is difficult due to the complexity of integrating MC light-paths in volumetric media with none or smooth material boundaries.

在科学可视化中，我们所获得的体数据集经常是代表一些光学上的不同物理属性的单值。通常没有可行的方法可以从这样的数据中获得发射和吸收属性。因此用户必须采用某种映射方法给数据值分配光学属性值来决定数据中的不同结构的模样。这类的映射就被称作传输函数。寻找合适的传输函数的过程就叫做分类。
