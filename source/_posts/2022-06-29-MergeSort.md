---
title: MergeSort
date: 2022-06-29 18:00:00
categories:
tags: 
- [C++, Algorithm]
tags:
- array
---

``` C++
#pragma once

#include <vector>

using i64 = long long;

template <typename T>
T MergeSortDo(int left, int right, std::vector<T> &a, std::vector<T> &b) {
    int mid = (left + right) / 2;
    int i = left, j = mid + 1, k = left;
    T res = 0;
    while (i <= mid && j <= right) {
        if (a[i] <= a[j]) {
            b[k] = a[i];
            i++, k++;
        } else {
            res += (j - k);
            b[k] = a[j];
            j++, k++;
        }
    }
    while (i <= mid) {
        b[k] = a[i];
        i++, k++;
    }
    while (j <= right) {
        b[k] = a[j];
        j++, k++;
    }
    for (int i = left; i <= right; i++) {
        a[i] = b[i];
    }
    return res;
}

template <typename T>
T MergeSortDFS(int left, int right, std::vector<T> &a, std::vector<T> &b) {
    T res = 0;
    if (left < right) {
        int mid = (left + right) / 2;
        res += MergeSortDFS<T>(left, mid, a, b);
        res += MergeSortDFS<T>(mid + 1, right, a, b);
        res += MergeSortDo<T>(left, right, a, b);
    }
    return res;
}

template <typename T>
T MergeSort(std::vector<T> &a) {
    std::vector<T> b(a);
    return MergeSortDFS<T>(0, a.size() - 1, a, b);
}

```