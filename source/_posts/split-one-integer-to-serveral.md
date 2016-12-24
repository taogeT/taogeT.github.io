---
title: 随机拆分一个正数为任意份
tags:
  - skill
  - search
categories: 研究心得
date: 2016-12-24 16:40:43
---


## 问题提出

设一个正数sum，需将其随机分为n份，其和仍保持为sum。

<!-- more -->

## 思路

思考线索如下：

1. 先转为正整数问题，浮点数可以转为整数*10^m(m<0) 替换处理。
1. 问题转变：假设存在sum个点排为一列，则存在sum-1个间隔。往间隔中插入n-1个分割板，将点分为n份。
1. 解决步骤：随机抽取n-1个小于sum的数组成数组，进行顺序排序，并在头部加0，尾部加sum。每个数之间的差(分割板位置)即可得到相加为sum的n份数据。
1. 如果为浮点数需要复原。

## 代码(Python)

    ```python
    # !/usr/env/bin python
    # -*- coding: utf-8 -*-
    from decimal import Decimal
    
    import random
    
    
    def split_integer(prototype, part):
        if not (isinstance(prototype, int) and isinstance(part, int)):
            raise ValueError('Input params should be integer.')
        if part <= 1:
            raise ValueError('Split part should be more than 1.')
        if prototype <= part:
            raise ValueError('Split part should be more than split prototype.')
        board_set = set()
        while len(board_set) < part - 1:
            board_set.add(random.randrange(1, prototype))
        board_list = list(board_set)
        board_list.append(0)
        board_list.append(prototype)
        board_list.sort()
        return [board_list[i + 1] - board_list[i] for i in range(part)]
    
    
    def split_float(prototype, part):
        if not isinstance(prototype, float):
            raise ValueError('Input prototype should be float.')
        if not isinstance(part, int):
            raise ValueError('Input part should be integer.')
        prototype = Decimal(str(prototype))
        _, decimal_digits, decimal_exponent = prototype.as_tuple()
        union_int = Decimal('0')
        pow_base = Decimal('10')
        for i, d in enumerate(decimal_digits):
            union_int += Decimal(str(d)) * pow_base ** (len(decimal_digits) - 1 - i)
        return [float(v * pow_base ** decimal_exponent) for v in split_integer(int(union_int), part)]
    
    
    if __name__ == '__main__':
        ''' integer sample '''
        part = random.randrange(2, 10)
        sample_int = random.randrange(20, 100)
        print(part, sample_int)
        res_int = split_integer(sample_int, part)
        print(res_int)
    
        ''' float sample '''
        part = random.randrange(2, 10)
        sample_float = round(random.uniform(20, 100), 8)
        print(part, sample_float)
        res_float = split_float(sample_float, part)
        print(res_float)
    
    ```

