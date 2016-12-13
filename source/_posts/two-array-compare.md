---
title: 快速查找两个数组中相同浮点数
date: 2016-12-09 23:11:02
tags:
- skill
- search
categories: 研究心得
---

## 问题提出

存在两个浮点数数组A、B，如何能快速查找到：

1. 浮点数x在数组A、B中都存在。(1:1)
2. 数组A存在浮点数y，与数组B中浮点数y1、y2....yn相加值相等。同样也在数组B中作相同查找。(1:N or N:1)

<!-- more -->

## 数据特征

数组的情况和解决方式要求为：

* 数组长度不会超过10000，但也不会低于1000。
* 两数组长度不等。
* 80%-90%的浮点数是符合1:1的情况。剩下的浮点数中都为1:N组合。
* 查找时间控制在1s内。

## 思路

思考线索如下：

1. 是否存在通用方法解决三种查找情况？不存在，由全组合公式可知存在的组合数量：2的N次方-1(N为数组长度)。由于数组长度不可采用此方式。
2. 按照先后顺序过滤？先查1:1，再查1:N，最后全组合剩余查找M:N。
3. 1:1怎么查？循环查找不可取，浮点数先对数组正序排序，再按照数组顺序查找相同元素。
4. 1:N怎么查？过滤完1:1元素，数组仍然是正序排列。按顺序选取数组A中元素a，查找数组B中所有小于a的浮点数族为数组b，对b进行全组合后相加，得到结果与a比较。对数组B也采取相同方式。

## 代码(Python)

    # !/usr/env/bin python
    # -*- coding: utf-8 -*-
    from collections import deque
    
    import math
    
    
    def create_mix_group_result(group):
        result = {}
        for index in range(int(math.pow(2, len(group) - 1) + 1), int(math.pow(2, len(group)))):
            binstr = bin(index)[2:]
            mix_group = [group[binindex] for binindex, binvalue in enumerate(binstr) if binvalue == '1']
            result[binstr] = sum(mix_group)
        return result
    
    
    def find_one_to_one(portalA, portalB):
        if len(portalA) <= 0 or len(portalB) <= 0:
            return None, portalA, portalB
    
        OneToOne, A_filter, B_filter = [], [], []
    
        A = deque(sorted(portalA))
        B = deque(sorted(portalB))
        while len(A) > 0 and len(B) > 0:
            if A[0] == B[0]:
                OneToOne.append(A[0])
                A.popleft()
                B.popleft()
            elif A[0] < B[0]:
                A_filter.append(A[0])
                A.popleft()
            else:
                B_filter.append(B[0])
                B.popleft()
        else:
            if len(A) > 0:
                A_filter.extend(A)
            if len(B) > 0:
                B_filter.extend(B)
    
        return OneToOne, A_filter, B_filter
    
    
    def find_one_to_many(portalA, portalB):
        if len(portalA) <= 0 or len(portalB) <= 0:
            return None, portalA, portalB
    
        OneToMany, A_filter, B_filter = [], [], []
    
        A = deque(portalA)
        B_mix_group = create_mix_group_result(portalB)
        while len(A) > 0:
            A_ele = A.popleft()
            for B_mix_key, B_mix_value in B_mix_group.items():
                if A_ele == B_mix_value:
                    newportalB = []
                    getB = []
                    for delindex, delstr in enumerate(B_mix_key):
                        if delstr == '1':
                            getB.append(portalB[delindex])
                        else:
                            newportalB.append(portalB[delindex])
                    OneToMany.append((A_ele, getB))
                    portalB = newportalB
                    B_mix_group = create_mix_group_result(portalB)
                    break
            else:
                A_filter.append(A_ele)
    
        B = deque(portalB)
        A_mix_group = create_mix_group_result(A_filter)
        while len(B) > 0:
            B_ele = B.popleft()
            for A_mix_key, A_mix_value in A_mix_group.items():
                if B_ele == A_mix_value:
                    newA_filter = []
                    getA = []
                    for delindex, delstr in enumerate(A_mix_key):
                        if delstr == '1':
                            getA.append(A_filter[delindex])
                        else:
                            newA_filter.append(A_filter[delindex])
                    OneToMany.append((getA, B_ele))
                    A_filter = newA_filter
                    A_mix_group = create_mix_group_result(A_filter)
                    break
            else:
                B_filter.append(B_ele)
    
        return OneToMany, A_filter, B_filter
    
    
    if __name__ == '__main__':
        A = [9, 1, 4, 6, 3, 10, 9, 3, 5, 2, 4, 10, 13]
        B = [3, 10, 2, 9, 6, 6, 1, 1, 4, 8, 3, 5, 8, 12, 2, 2]
    
        OneToOne, A_filter, B_filter = find_one_to_one(A, B)
        print(OneToOne, A_filter, B_filter)
        OneToMany, A_filter, B_filter = find_one_to_many(A_filter, B_filter)
        print(OneToMany, A_filter, B_filter)
    






















