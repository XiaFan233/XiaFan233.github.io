---
title: Segment Tree
date: 2022-06-04 18:00:00
categories:
- [C++, Algorithm]
tags:
- tree
---

``` C++
#include <cassert>
#include <iostream>
#include <vector>

template <typename T> class SegmentTree {
private:
    struct node {
        T val;
        T tag;
        int cnt;  // numbers can be modify
        node() : val(0), tag(0), cnt(0) {}
    };
    int n;
    std::vector<node> tree;
    int ls(int root) {
        return root * 2 + 1;
    }
    int rs(int root) {
        return root * 2 + 2;
    }

    void push_up(int root) {
        tree[root].val = tree[ls(root)].val + tree[rs(root)].val;
        // tree[root].cnt = tree[m_l(root)].cnt + tree[m_r(root)].cnt;
    }

    void build(int root, int left, int right, const std::vector<T> &num) {
        assert(root < 4 * n);
        if (left == right) {
            tree[root].val = num[left];
            // tree[root].cnt = 1;
            return;
        }
        int mid = (left + right) / 2;
        build(ls(root), left, mid, num);
        build(rs(root), mid + 1, right, num);
        push_up(root);
    }

    void change(int root, int left, int right, T val) {
        tree[root].tag += val;
        tree[root].val += (val * (right - left + 1));
        // tree[root].val += (val * tree[root].cnt);
    }

    void push_down(int root, int left, int right) {
        int mid = (left + right) / 2;
        change(ls(root), left, mid, tree[root].tag);
        change(rs(root), mid + 1, right, tree[root].tag);
        tree[root].tag = 0;
    }

    void update(int root, int left, int right, int l, int r, int val) {
        assert(root < 4 * n);
        if (l <= left && right <= r) {
            change(root, left, right, val);
            return;
        }
        push_down(root, left, right);
        int mid = (left + right) / 2;
        if (l <= mid) update(ls(root), left, mid, l, r, val);
        if (mid + 1 <= r) update(rs(root), mid + 1, right, l, r, val);
        push_up(root);
    }

    void cnt_update(int root, int left, int right, int pos, int val) {
        assert(root < 4 * n);
        tree[root].cnt += val;
        if (left == right) {
            return;
        }
        push_down(root, left, right);
        int mid = (left + right) / 2;
        if (pos <= mid) {
            cnt_update(ls(root), left, mid, pos, val);
        } else {
            cnt_update(rs(root), mid + 1, right, pos, val);
        }
        push_up(root);
    }

    T query(int root, int left, int right, int l, int r) {
        assert(root < 4 * n);
        if (l <= left && right <= r) {
            return tree[root].val;
        }
        T ans = 0;
        push_down(root, left, right);
        int mid = (left + right) / 2;
        if (l <= mid) {
            ans += query(ls(root), left, mid, l, r);
        }
        if (mid + 1 <= r) {
            ans += query(rs(root), mid + 1, right, l, r);
        }
        return ans;
    }

public:
    SegmentTree(cosnt std::vector<T> &num) : n(num.size()), tree(num.size() * 4) {
        build(0, 0, n - 1, num);
    }

    void update(int left, int right, int val) {
        update(0, 0, n - 1, left, right, val);
    }

    T query(int left, int right) {
        return query(0, 0, n - 1, left, right);
    }

    void cnt_update(int pos, int val) {
        assert(val == -1 || val == 1);
        push_down(0, 0, n - 1);
        cnt_update(0, 0, n - 1, pos, val);
    }
};
```