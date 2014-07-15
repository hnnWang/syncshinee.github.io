---
layout: page
title: "Source of D Who Is Joyful"
date: 2014-07-14 22:35
comments: true
sharing: true
footer: true
---
```c++
#include<cstdio>
#include<cstring>
#include<stack>
#include<vector>
#include<algorithm>
using namespace std;

struct node{
	int l, r, ind;
}qry[100010];

int C[100010], n, high[100010], pre[100010], nxt[100010], ans[100010];
stack<int> st;
vector<int> frt[100010];

void update(int x, int del) {
	if (x == 0)
		return;
	while (x <= n) {
		C[x] += del;
		x += x & -x;
	}
}

int getsum(int x) {
	int sum = 0;
	while (x > 0) {
		sum += C[x];
		x -= x & -x;
	}
	return sum;
}

bool cmp(node x, node y) {
	return x.r < y.r;
}

int main() {
	int T, Q, x, y;
	scanf("%d", &T);
	for (int cas = 1; cas <= T; ++cas) {
		memset(C, 0, sizeof(C));
		printf("Case %d:\n", cas);
		scanf("%d", &n);
		high[0] = -1;
		high[n+1] = 2000000000;
		for (int i = 1; i <= n; ++i) {
			frt[i].clear();
			scanf("%d", &high[i]);
		}
		//单调栈预处理出所有点的前面第一个比它小的和后面第一个比他大的
		while (!st.empty())
			st.pop();
		st.push(0);
		for (int i = 1; i <= n; ++i) {
			while (!st.empty() && high[i] <= high[st.top()])
				st.pop();
			pre[i] = st.top();
			st.push(i);
		}

		while (!st.empty())
			st.pop();
		st.push(n+1);
		for (int i = n; i > 0; --i) {
			while (!st.empty() && high[i] >= high[st.top()])
				st.pop();
			nxt[i] = st.top();
			st.push(i);
		}
		//对每一个点找出所有以他为三元组结尾元素的三元组中间元素
		for (int i = 1; i <= n; ++i)
			frt[nxt[i]].push_back(i);
		//读入询问并排序
		scanf("%d", &Q);
		for (int i = 0; i < Q; ++i) {
			scanf("%d%d", &x, &y);
			if (x > y) {
				x = x^y;
				y = x^y;
				x = x^y;
			}
			qry[i].l = x;
			qry[i].r = y;
			qry[i].ind = i;
		}
		sort(qry, qry+Q, cmp);

		//按顺序使用树状数组更新并求解
		int rr = 0;
		for (int i = 0; i < Q; ++i) {
			while (rr < qry[i].r) {
				++rr;
				for (int j = 0; j < frt[rr].size(); ++j)
					update(pre[frt[rr][j]], 1);
			}
			ans[qry[i].ind] = getsum(qry[i].r) - getsum(qry[i].l - 1);
		}

		for (int i = 0; i < Q; ++i)
			printf("%d\n", ans[i]);
	}
	return 0;
}
```
[返回](/blog/2014/07/14/bupt0solution/)