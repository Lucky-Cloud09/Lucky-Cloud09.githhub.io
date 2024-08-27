---
title: 2023 10月杂题记
categories: 杂题
date: 2023/10/02
cover: https://lucky-cloud09.github.io/img/b3.png
---


# [CF1875D](https://www.luogu.com.cn/problem/CF1875D)

我们经过思考，容易得出以下结论：

1. 如果当前 $mex = x$，则下一个删的数一定小于 $x$。
1. 如果 $mex = 0$，那么我们就可以不往下算了，因为它们对答案的贡献为 $0$。

我们设 $f[i]$ 表示当 $mex = i$ 时，$m$ 的值。

则有：

$$f[i] = \min(f[j] + (c[i] - 1) \times j + i, f[i])$$

其中 $j > i$，$c[i]$ 表示 $i$ 在 $a$ 中的个数。

因为，我们要使 $mex = i$，就必须将 $i$ 这个数删去，并且 $0 \sim i-1$ 都还存在于 $a$ 中。我们会删 $c[i]$ 次，但 $c[i] - 1$ 次，$m$ 会加上上一个 $mex$ 的值。 第 $c[i]$ 次则会加上 $i$，也就是新的 $mex$。

设没删任何数的 $mex = first$。

根据定义，初始化 $f[first] = 0$。

根据上述结论，与定义，答案即为 $f[0]$。

# [CF755F](https://www.luogu.com.cn/problem/CF755F)

不想腾 markdown 了，[点这里吧](https://www.luogu.com.cn/blog/712506/solution-cf755f)。

# [P2816](https://www.luogu.com.cn/problem/P2816)

我认为不值得绿，如果没有加强数据，黄就差不多得了。

很简单的贪心。与导弹拦截差不多相同。就是枚举当前有多少堆，再判断一下取最小值。当然判断即为贪心。每一堆新加一个一定要尽可能大。

时间复杂度为 $O(n^2)$，考虑优化，想一下，明显的有单调性，可以选择二分答案，时间复杂度 $O(n \log n)$，瓶颈在于排序与二分。

代码：
```cpp
#include <bits/stdc++.h>
using namespace std;

int n, a[5100], ans = 1e9, cnt, v[5100];

bool check(int x) {
	for (int i = 0; i <= x; i++) v[i] = 1e9;
	for (int i = 1; i <= n; i++) v[i % x] = min(v[i % x] - 1, a[i]);
	for (int i = 0; i <= x; i++)
		if (v[i] < 0) return 0;
	return 1;
}

int main() {
	cin >> n;
	for (int i = 1; i <= n; i++) {
		cin >> a[i];
	}
	sort(a + 1, a + 1 + n, [](int x, int y){return x > y;});
	int l = 1, r = n;
	while (l <= r) {
		int mid = (l + r) >> 1;
		if (check(mid)) {
			ans = min(ans, mid);
			r = mid - 1;
		}
		else l = mid + 1;
	}
	cout << ans;
	return 0;
}
```