---
title: Merge Sort
date: 2022-06-29 18:00:00
categories:
- [C++, Algorithm]
tags:
- Array
- Sort
---

``` C++
#include <cassert>
#include <iostream>
#include <vector>

constexpr int mod = 1e9;

template <typename T, typename U>
T MergeSortDo(int left, int right, std::vector<U> &a, std::vector<U> &b) {
    int mid = (left + right) / 2;
    int i = left, j = mid + 1, k = left;
    T res = 0;
    while (i <= mid && j <= right) {
        if (a[i] <= a[j]) {
            b[k] = a[i];
            i++, k++;
        } else {
            res = (res + (j - k)) % mod;
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

template <typename T, typename U>
T MergeSortDFS(int left, int right, std::vector<U> &a, std::vector<U> &b) {
    T res = 0;
    if (left < right) {
        int mid = (left + right) / 2;
        res = (res + MergeSortDFS<T, U>(left, mid, a, b)) % mod;
        res = (res + MergeSortDFS<T, U>(mid + 1, right, a, b)) % mod;
        res = (res + MergeSortDo<T, U>(left, right, a, b)) % mod;
    }
    return res;
}

template <typename T, typename U>
T MergeSort(std::vector<U> &a) {
    std::vector<U> b(a);
    return MergeSortDFS<T, U>(0, a.size() - 1, a, b);
}

```