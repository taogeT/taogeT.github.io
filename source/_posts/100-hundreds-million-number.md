---
title: 海量数据中快速找到最大的100个
tags:
- skill
- search
categories: 研究心得
date: 2016-12-14 05:38:09
---


## 问题提出

存在五十万个数字，如何快速找到最大(小)的100个。

<!-- more -->

## 思路

思考线索如下：

1. 普通排序方式，需要同时把数据全部加载到内存排序，由于python性能问题不采用。
2. 采用堆排序(heapq)的方式提升排序速度。
3. 不全部加载，每次取500个数据，取其中最大100个。然后再以每次500个加载比较。

## 代码(Python)

    ```python
    # !/usr/env/bin python
    # -*- coding: utf-8 -*-
    import heapq
    import random
    
    
    if __name__ == '__main__':
        sample = []
        for _ in range(1000):
            sample += [random.randint(1, 100000) for _ in range(500)]
            sample = heapq.nlargest(100, sample)
        print(sample)
    ```