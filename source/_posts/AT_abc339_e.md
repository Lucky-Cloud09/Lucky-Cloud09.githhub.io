---
title: AT_abc339_e
cover: https://lucky-cloud09.github.io/img/b3.jpg
categories: 题解
---


传送门。

## 题意
给你一个长度为 $N$ 的序列 $A = (A_1, A_2, \ldots, A_N)$。

求长度为 $A$ 的子序列的最大长度，使得任意两个相邻项之间的绝对差最多为 $D$。

## 分析

设 $f[i]$ 为以 $A$ 中值为 $i$ 为结尾的子序列的最大长度。则有：

$$f[i] = \max(f[j]) + 1$$

其中 $i - d \le j \le i + d$。

代码很容易出来：

```cpp
for (int i = 1; i <= n; ++i) {
	int x = 0;
	for (int j = max(a[i] - d, 1); j <= min(a[i] + d, mx); ++j) x = min(f[j] + 1, x);
	f[a[i]] = x;
}
```
$mx$ 为 $A$ 中的最大值。

复杂度 $O(n^2)$。我们不能接受。

其中这一段：

```cpp
for (int j = max(a[i] - d, 1); j <= min(a[i] + d, mx); ++j) x = min(f[j] + 1, x);
```

相当于在找一个区间内的 $f$ 最大值。

但是，考虑到 $f[i]$ 会变化。我们使用线段树板子维护这个最大值。

时间复杂度：$O(n\log n)$

## 代码

```cpp
#include <bits/stdc++.h>
#define int long long
#define ls (p << 1)
#define rs ((p << 1) + 1)
using namespace std;

const int N = 5e5 + 5;
int n, d, f[N], a[N];
struct tree {
	int l, r;
	int mx;
} tr[N * 4];

void pushup(int p) {
	tr[p].mx = max(tr[ls].mx, tr[rs].mx);
}

void build(int l, int r, int p) {
	tr[p].l = l, tr[p].r = r, tr[p].mx = 0;
	if (l == r) { return ; }
	int mid = (l + r) >> 1;
	build(l, mid, ls);
	build(mid + 1, r, rs);
}

void change(int x, int p) {
	int l = tr[p].l, r = tr[p].r;
	if (l == r) {
		tr[p].mx = f[x];
		return ;
	}
	int mid = (l + r) >> 1;
	if (x <= mid) change(x, ls);
	else change(x, rs);
	pushup(p);
}

int que(int L, int R, int p) {
	int l = tr[p].l, r = tr[p].r;
	if (L <= l && r <= R) return tr[p].mx;
	int mid = (l + r) >> 1;
	int res = -1e9;
	if (L <= mid) res = max(res, que(L, R, ls));
	if (R > mid) res = max(res, que(L, R, rs));
	return res;
}

signed main() {
	cin >> n >> d;
	int mx = 0;
	for (int i = 1; i <= n; ++i) cin >> a[i], mx = max(mx, a[i]);
	build(1, mx, 1);
	int ans = -1e9;
	for (int i = 1; i <= n; ++i) {
		int x = a[i];
		f[x] = que(max(1ll, x - d), min(mx, x + d), 1) + 1;
		change(x, 1);
		ans = max(ans, f[x]);
	}
	cout << ans;
	return 0;
}
```