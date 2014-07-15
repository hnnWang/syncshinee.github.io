---
layout: page
title: "Source of    C Freeway"
date: 2014-07-14 22:35
comments: true
sharing: true
footer: true
---
```c++
//bfs版本
#include <cstdio>
#include <vector>
#include <cstring>
#include <queue>
using namespace std;
struct Edge{
	int t, v, w;
}edge[200010];
struct node{
	int ind, val;
}na, nb;

vector<int> gra[100010];
queue<node> q;
queue<int> qu;
bool vis[100010];
int side, minl, ans;

void build(int t, int u, int v, int w) {
	edge[++side].t = t;
	edge[side].v = v;
	edge[side].w = w;
	gra[u].push_back(side);
}

int main() {
	int n, t, u, v, w, tmp;
	while (scanf("%d", &n) != EOF) {

		side = 0;
		memset(vis, 0, sizeof(vis));
		for (int i = 0; i < 100010; ++i)
			gra[i].clear();
		//建图
		for (int i = 0; i < n-1; ++i) {
			scanf("%d%d%d%d", &t, &u, &v, &w);
			if (t == 0) {
				build(0, u, v, w);
				build(0, v, u, w);
			}
			else {
				build(1, u, v, w);
				build(-1, v, u, w);
			}
		}
		//找到图的一个点
		int k = 1;
		while (gra[k].size() == 0 && k < 100010)
			k++;
		//bfs求出从这个点出发的费用
		vis[k] = true;
		qu.push(k);
		minl = 0;
		while (!qu.empty()) {
			tmp = qu.front();
			qu.pop();
			for (int i = 0; i < gra[tmp].size(); ++i) {
				if (! vis[edge[gra[tmp][i]].v]) {
					vis[edge[gra[tmp][i]].v] = true;
					if (edge[gra[tmp][i]].t == -1)
						minl += edge[gra[tmp][i]].w;
					qu.push(edge[gra[tmp][i]].v);
				}
			}
		}
		ans = k;
		//bfs求出所有点的费用,取最小
		memset(vis, 0, sizeof(vis));
		na.ind = k;
		na.val = minl;
		vis[k] = true;
		q.push(na);
		while (!q.empty()) {
			na = q.front();
			q.pop();
			for (int i = 0; i < gra[na.ind].size(); ++i) {
				if (!vis[edge[gra[na.ind][i]].v]) {
					vis[edge[gra[na.ind][i]].v] = true;
					nb.ind = edge[gra[na.ind][i]].v;
					if (edge[gra[na.ind][i]].t == 0) {
						nb.val = na.val;
					} else if (edge[gra[na.ind][i]].t == 1) {
						nb.val = na.val+edge[gra[na.ind][i]].w;
					} else {
						nb.val = na.val-edge[gra[na.ind][i]].w;
					}
					if (nb.val < minl) {
						minl = nb.val;
						ans = nb.ind;
					}
					else if (nb.val == minl && nb.ind < ans)
						ans = nb.ind;
					q.push(nb);
				}
			}
		}
		printf("%d %d\n", ans, minl);
	}	
	return 0;
}

//dfs版本
#include <cstdio>
#include <vector>
#include <cstring>
using namespace std;

struct Edge{
    int t, v, w;
}edge[200010];

vector<int> gra[100010];
bool vis[100010];
int side, minl, ans;

void build(int t, int u, int v, int w) {
    edge[++side].t = t;
    edge[side].v = v;
    edge[side].w = w;
    gra[u].push_back(side);
}

int dfs(int x) {
    int tmp = 0;
    vis[x] = true;
    for (int i = 0; i < gra[x].size(); ++i){
        if (!vis[edge[gra[x][i]].v]) {
            if (edge[gra[x][i]].t > -1)
                tmp += dfs(edge[gra[x][i]].v);
            else
                tmp += dfs(edge[gra[x][i]].v)+edge[gra[x][i]].w;
        }
    }
    return tmp;
}
  
void solve(int k, int val) {
    vis[k] = true;
    if (val < minl) {
        minl = val;
        ans = k;
    }
    else if (val == minl && k < ans)
        ans = k;
    for (int i = 0; i < gra[k].size(); ++i) {
        if (!vis[edge[gra[k][i]].v]) {
            if (edge[gra[k][i]].t == 0)
                solve(edge[gra[k][i]].v, val);
            else if (edge[gra[k][i]].t == 1)
                solve(edge[gra[k][i]].v, val + edge[gra[k][i]].w);
            else
                solve(edge[gra[k][i]].v, val - edge[gra[k][i]].w);
        }
    }
}
int main() {
    int n, t, u, v, w;
    while (scanf("%d", &n) != EOF) {
        side = 0;
        memset(vis, 0, sizeof(vis));
        for (int i = 0; i < 100010; ++i)
            gra[i].clear();
		//建图
        for (int i = 0; i < n-1; ++i) {
            scanf("%d%d%d%d", &t, &u, &v, &w);
            if (t == 0) {
                build(0, u, v, w);
                build(0, v, u, w);
            }
            else {
                build(1, u, v, w);
                build(-1, v, u, w);
            }
        }
		//找到图的一个点
        int k = 1;
        while (gra[k].size() == 0)
            k++;
		//dfs求出从这个点出发的费用
        minl = dfs(k);
        ans = k;
		//dfs求出所有点的费用,取最小
        memset(vis, 0, sizeof(vis));
        solve(k, minl);

        printf("%d %d\n", ans, minl);
    } 
    return 0;
}
```
[返回](/blog/2014/07/14/bupt0solution/)