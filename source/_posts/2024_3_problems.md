---
title: 2024 3月杂题记
cover: https://lucky-cloud09.github.io/img/b7.jpg
categories: 杂题
date: 2024/3
---


过了几个月，又回来了，3.7 之前的懒得补了。

## 3.7

### P2487 [SDOI2011] 拦截导弹

最近在学 CDQ。花了我好久调试。

CDQ 优化 DP 模板。

将转移条件转化成三维偏序。在 CDQ 中求。至于每个点在最长的二维最长升子序列的出现次数，多开一个数组 $f[0/1][i]$ 存，转移还是使用树状数组顺带做了。$0/1$，表示以此点为开头或结尾。

最后如果 $dp[0][i] + dp[1][i] = len + 1$ 则这个点答案为 $f[0][i] * f[1][i] / sum$。


### P3834 可持久化线段树 2

顺带用整体二分做了一个板子题。感觉这个算法好 NB。

### P4093 [HEOI2016/TJOI2016] 序列

一开始连题都看错了。~~描述不清的题面，SM 的出题人。~~或许是我是个 SB。

开了题解才明白题目。~~是我大意了。~~

也是一道很板的 CDQ 优化 DP。但以后一定要注意，转移时是 `dp[a[i].id]` 而不是 `dp[i]`。（-1h。

### P3364 Cool loves touli

板子 +1。

注意开 `long long`，然后再离散化。树状数组与离散化数组大小开 $3 \times N$。我在这里 WA 了一发。

### P4390 [BalkanOI2007] Mokia 摩基亚

简单板子。将查询分为四个点每次求 0,0 到 x,y 的一个矩形中的总权值。

相当于求 $x_i \le x_j \land y_i \le y_j \land t_i \le t_j$ 的 $a_i$ 总和。三维偏序，可以使用 CDQ 维护它。

注意树状数组上界一定要开大，不然会少加上数！

## 3.8

### CF1045G AI robots

完蛋，板子题做多了，脑袋不灵了。。。

在处理中间部分时利用尺取法维护能满足智商条件的 bot。

先按 $r$，再按 $q$ 排。

注意离散化和数组大小。

### P3769 [CH弱省胡策R2] TATT

四维偏序板子。CDQ 套 CDQ 套 树状数组即可。

每一个均维护一种偏序。最外层其实也有排序保证。

## 3.11

### P3527 [POI2011] MET-Meteors

整体二分板子。尽量不要下传数组。还有要将目前的空间站离散。

### P4870 [BalticOI 2009 Day1] 甲虫

区间 DP。设 $f[i][j][0/1]$ 表示，$i\sim j$ 全吃，当前在左/右。随便转移一下即可。

### P4158 [SCOI2009] 粉刷匠

要预处理出每一块木板用不同次粉刷最多粉刷多少。

$$f[i][j][k] = \max(f[i][j][k], f[i][l][k - 1] + \max(sum[i][j] - sum[i][l], j - l - sum[i][j] + sum[i][l]));$$

$sum$ 存颜色的前缀和。

$dp[i][j] = max(dp[i][j], dp[i - 1][j - k] + f[i][m][k]);$。前 $i$ 个板刷 $j$ 次。

### P3174 [HAOI2009] 毛毛虫

简单去处理当前最大的毛毛虫与次大的毛毛虫，并简单的将他们两个合并。

### P1131 [ZJOI2007] 时态同步

找到当前子树最深的一个点。并使其他的叶子节点的深度与他相同，再计算贡献即可。

### P1272 重建道路

利用树上的背包去做即可。

$$f[x][s] = min(f[x][s] + 1, f[x][s - sy] + f[y][sy]);$$

$f$ 表示在 $x$ 的子树中留下 $s$ 个点的最少操作次数。

最后对所有 $f[i][p]$ 取 $min$ 就行。

### P3554 [POI2013] LUK-Triumphal arch

二分答案。利用树形 DP check 就可。

$f[i]$ 表示在 $i$ 的子树中还需从上面的节点借的个数。

### P6419 [COCI2014-2015#1] Kamp

换根 DP。先去处理出以 $1$ 为根。然后再计算其他的即可。答案为送完所有人走回根的答案减去含家的最长链即可。


## 3.12

### P4657 [CEOI2017] Chase

做了有两天了。还是得开题解。其实就是换根 DP。

考虑在当前节点放一个磁铁。那么贡献即为 $\sum f_{son_x}$。

$dp[x][i][1] = dp[x][i - 1][0] + sum[x]$。$dp[x][i][0] = \max(dp[son_x][i][0])$。

同时记录下儿子节点答案的最大值与次大值。

在换根时，套路的去做就可以了（如果他是父亲的最大值，就次大值更新此类）。

### P2495 [SDOI2011] 消耗战

使用虚树，再进行 DP 即可。

### P2986 [USACO10MAR] Great Cow Gathering G

换根 DP 板子。很容易想到，根作为聚集点。那么，我们先处理 $f[1]$。

$$f[1] = \sum_{1 < y \le n} dep[y] * c[y]$$

顺便处理 $sum_i$ 表示 $i$ 为根的子树内的牛数。

答案 $f[y] = f[x] + (sum[1] - sum[y]) \times w - sum[y] \times w;$。$w$ 为边权。

答案在 $f$ 中取 $\min$ 即可。


## 3.13

### P5665 [CSP-S2019] 划分

简单题，不知道为什么是紫。自然想到段数越多越好，也是最后一段越短越好。至于题面，很容易就想到 DP 了吧。

我们就只需要维护一个单调队列即可，注意要保存上一个转移而来的数。

## 3.14

### P7424 [THUPC2017] 天天爱射击

与这道题有点像：P3527 [POI2011] MET-Meteors。

就按照这么做即可。

### P1527 [国家集训队] 矩阵乘法

矩阵中的第 K 小。将一维第 K 小的树状数组换成二维树状数组，求前缀和变为求二维前缀和即可。

### P2839 [国家集训队] middle

二分 + 主席树板子。

二分 $mid$ 为中位数。$> mid$ 的标为 $1$，否则标为 $-1$。

建立主席树进行维护区间和，若和 $\ge 0$ 则 $mid$ 可以尝试变得更大。

考虑左右端点，那么对于 $a\sim b$ 我们要找到最大后缀和，$c\sim d$ 找到最大前缀和。

相加判断即可。

### P3899 [湖南集训] 更为厉害

线段树合并。考虑对每一个点建一个线段树，其中以深度为下标。

分析可得 $a,b$ 存在祖先关系。

$a,b$ 分为两种情况：

1. $a$ 在 $b$ 上。
1. $a$ 在 $b$ 下。

$a$ 在 $b$ 上 simple 求。

$a$ 在 $b$ 下就在线段树中找到符合条件的 $b$，当 $b$ 确定后，$c$ 就有 $b$ 的子树大小减一中可能。

最后两种情况答案相加即可。

## 3.15

### P8028 [COCI2021-2022#3] Cijanobakterije

模拟赛 T1。

刚开题发现就是求若干个树的直径总和。

写了半个小时发现自己不会写 DP。（汗

于是打了两次 BFS。

### P9032 [COCI2022-2023#1] Neboderi

模拟赛 T2。

赛时差点没做出来，后面又因为常数大了，T 掉了。（汗

利用倍增求一段区间的 gcd。

贪心地。很明显，固定 $r$，$l \sim r$ 的 gcd 单调不减。那么我们肯定是去第一次出现一个 gcd 的区间求值。

### P8313 [COCI2021-2022#4] Izbori

模拟赛 T3。

说实话，很妙。

根据简单地推导，满足 $2\times S_i - i > 2 \times S_j - j$ 的 $i$ 与 $j$ 即可有一个区间，其中 $S$ 为某种颜色出现的次数和。

很容易想到拿一个数据结构去维护。选择树状数组，但由于 $2\times S_i - i$ 可能为负，所以要加一个偏移量。

我们将 $2\times S_i - i$ 称为 $C_i$。

手玩一下容易发现：对于一种颜色在前缀中的 $C_i$ 在没有遇到相同颜色时，$C_i = C_{i - 1} - 1$ 否则 $C_i = C_{i - 1} + 1$。

那么我们枚举同一种颜色，相邻的可以视为一个区间，我们考虑计算与加上一个区间的贡献。

可以推出一个区间的贡献可以通过求每个 $C_i$ 贡献的前缀和再相减的方式求出。

用树状数组维护，我们支持加上区间的贡献，区间修改。再询问——单点查询。

那么再将所有区间贡献相加，即为答案。

## 3.28

### C0504 C 距离之和

> 题目大意：给一棵树。每次操作可以删一条边并再加一条边。求使的每个点到其余点距离之和在当前这棵树最小的最少操作次数。

点到其余点距离之和在当前这棵树最小，明显就是要使这个点成为一个中心。

我们先求出初始的重心 $rt$。

因为重心的每一棵子树大小 $\le \left \lfloor \frac{n}{2} \right \rfloor $。

那么我们假设考虑到一个点 $u$，并且我们再定义 $b_u$ 为距离 $u$ 最近的 $rt$ 的儿子（包括 $u$），我们可以得到 $siz_u \le siz_{b_u} \le \left \lfloor \frac{n}{2} \right \rfloor$，$siz_i$ 是在以 $rt$ 为根的树中 $i$ 为根的子树大小。

我们将点 $u$ 视为当前的根节点，那么除了它在原树中的子树，还有一个大小为 $n - siz_u$ 的子树。

因为 $siz_u \le \left\lfloor\frac{n}{2}\right\rfloor$ 所以 $n - siz_u \ge \left\lfloor\frac{n}{2}\right\rfloor$，我们就要去调整它的大小。

我们选择叫 $n - siz_u$ 这棵子树为大子树。

我们设 $now = n - siz_u - \left\lfloor\frac{n}{2}\right\rfloor$，这个就是要从大子树中至少要减去子树的大小的下界。

我们要减的子树一定是以 $rt$ 儿子为根的子树，我们可以减去 $rt$ 到它儿子的边，然后可以连向 $u$。

当然 $b_u$ 为根的子树大小要变为 $size_{b_u} - siz_u$。

我们考虑将他们加入一个数据结构，并找到子树大小的和刚好大于等于 $now$ 的最小个数。

在这里我选择使用权值树状数组 + 二分。

```cpp
#include <bits/stdc++.h>
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
const int N = 1e6 + 5;
int n, root, siz[N];
int b[N], ans[N], sum1[N], sum2[N];
vector<int> g[N];

void get_rt(int x, int fa) {
	siz[x] = 1;
	bool flag = 1;
	for (int y : g[x]) {
		if (y == fa) continue;
		get_rt(y, x);
		if (siz[y] > n / 2) flag = 0;
		siz[x] += siz[y];
	}
	if (n - siz[x] > n / 2) flag = 0;
	if (flag) root = x;
}

//void build(in)

void dfs1(int x, int fa, int tag) {
	b[x] = tag;
	siz[x] = 1;
	for (int y : g[x]) {
		if (y == fa) continue;
		dfs1(y, x, tag);
		siz[x] += siz[y];
	}
}

void upd(int x, int v1, int v2) {
	x++;
	for (; x <= n + 1; x += x & -x) 
		sum1[x] += v1, sum2[x] += v2;
}

int ask1(int x) {
	x++;
	int res = 0;
	for (; x; x -= x & -x)
		res += sum2[x];
	return res;
}
int ask(int x) {
	x++;
	int res = 0;
	for (; x; x -= x & -x)
		res += sum1[x];
	return res;
}

int getans(int x) {
	upd(siz[b[x]], -siz[b[x]], -1);
	upd(siz[b[x]] - siz[x], siz[b[x]] - siz[x], 1);
	int now = (n - siz[x] - (n >> 1));
	if (now <= 0 && siz[x] <= (n >> 1)) return 0;
	int l = 1, r = n, res = 0, all = ask(n);
	while (l <= r) {
		int mid = (l + r) >> 1;
		if (all - ask(mid - 1) >= now) l = mid + 1, res = mid;
		else r = mid - 1;
	}
	int a1 = ask(res);
	int t = now - (all - a1);
	int t1 = (t - 1) / res + 1;
//	cout << b[x] << ' '<< x << ' ' << res << ' ' << now << ' ' << ask(n) - ask(res - 1) << ' ';

	res = t1 + ask1(n) - ask1(res);
	
	upd(siz[b[x]], siz[b[x]], 1);
	upd(siz[b[x]] - siz[x], - siz[b[x]] + siz[x], -1);
	return res;
}

int main() {
	n = read();
	for (int i = 1; i < n; ++i) {
		int u = read(), v = read();
		g[u].push_back(v);
		g[v].push_back(u);
	}
//	puts("");
	get_rt(1, 0);
	for (int x : g[root]) dfs1(x, root, x);
	
	for (int x : g[root]) 
		upd(siz[x], siz[x], 1);
	
	for (int i = 1; i <= n; ++i) 
		if (i != root) 
			ans[i] = getans(i);
	for (int i = 1; i <= n; ++i) cout << ans[i] << '\n';
	return 0;
}


```