---
title: Sparse Table
date: 2022-06-06 18:00:00
categories:
- [C++, Algorithm]
tags:
- Matrix
- Array
- Fast
---

``` C++
#include <cassert>
#include <cmath>
#include <functional>
#include <iostream>
#include <vector>

template <typename T, class F = std::function<T(const T&, const T&)>> 
class SparseTable {
private:
    int n;
    std::vector<std::vector<T>> mat;
    F func;

public:
    SparseTable(const std::vector<T>& a, const F& f) : n(int(a.size())), func(f) {
        assert(n > 0);
        // int max_log = 32 - __builtin_clz(n);
        int max_log = int(log2(n)) + 1;
        mat.resize(max_log);
        mat[0] = a;
        for (int i = 1; i < max_log; i++) {
            int m = n - (1 << i) + 1;
            mat[i].resize(m);
            for (int j = 0; j < m; j++) {
                mat[i][j] = func(mat[i - 1][j], mat[i - 1][j + (1 << (i - 1))]);
            }
        }
    }

    T get(int from, int to) const {
        assert(0 <= from && from <= to && to < n);
        // int lg = 32 - __builtin_clz(to - from + 1) - 1;
        int lg = int(log2(to - from + 1));
        return func(mat[lg][from], mat[lg][to - (1 << lg) + 1]);
    }
};
```