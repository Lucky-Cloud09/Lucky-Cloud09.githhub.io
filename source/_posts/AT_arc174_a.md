---
title: 题解：AT_arc174_a [ARC174A] A Multiply
cover: https://lucky-cloud09.github.io/img/b1.jpg
categories: 题解
---


[题传](https://www.luogu.com.cn/problem/AT_arc174_a)。

先要将 $C$ 分类。

1. $C > 0$，为了使答案更大，要乘上一个最大的区间和。
1. $C \le 0$，为了使答案更大，选择乘上一个最小的区间和，因为此时我们可以贪心地想，如果区间和越小，乘上一个负数或 $0$ 后，答案减少得越小，甚至乘上负数，还会使答案增大，所以也可以用负负得正来解释。

当然我们也可以不进行操作。

要求区间和，我们选择前缀和即可。

因为前缀和求区间 $l \sim r$ 的和是 $sum_r - sum_{l - 1}$。要求区间和的最值，我们固定 $sum_r$ 就可以求 $sum_{l - 1}$ 的最值，我们遍历一遍并动态维护一下区间和的最值即可。

注意 $i \sim i + 1$ 我们可以视为不选区间与 $C$ 相乘。

给一下代码：

```cpp
#include <bits/stdc++.h>
#define int long long
using namespace std;

inline int read() {
	int res = 0, f = 1; char c = getchar();
	while (c > '9' || c < '0') {
		if (c == '-') f = -1;
		c = getchar();
	}
	while (c >= '0' && c <= '9') {
		res = (res << 1) + (res << 3) + (c ^ 48);
		c = getchar();
	}
	return f == 1 ? res : -res;
}

int n, c, a[(int)3e5 + 5], sum[(int)3e5 + 5];

signed main() {
	n = read(), c = read();
	for (int i = 1; i <= n; ++i) 
		a[i] = read(), sum[i] = sum[i - 1] + a[i];
	if (c > 0) {
		int ans = -1e18, mi = 0;
		for (int i = 1; i <= n; ++i) {
			mi = min(sum[i], mi);//求 sum[i-1] 的最值，下同。
			ans = max(ans, sum[i] - mi);//求和的最值，下同。
		}
		cout << sum[n] - ans + c * ans;//计算答案下同。
	}
	else {
		int ans = 1e18, mx = 0;
		for (int i = 1; i <= n; ++i) {
			mx = max(sum[i], mx);
			ans = min(ans, sum[i] - mx);
		}
		cout << sum[n] - ans + c * ans;
	}
	return 0;
}

```