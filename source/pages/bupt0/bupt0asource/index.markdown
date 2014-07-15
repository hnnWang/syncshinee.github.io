---
layout: page
title: "Source of    A Hard Game"
date: 2014-07-14 22:35
comments: true
sharing: true
footer: true
---
```c++
#include<cstdio>
#include<cstring>
using namespace std;
int main(){
	int T, n, m;
	scanf("%d", &T);
	while (T--) {
		scanf("%d%d", &n, &m);
			if ((n & 1) == 1)
				printf("Alice\n");
			else 
				printf("Bob\n");
	}
	return 0;
}
```
[返回](/blog/2014/07/14/bupt0solution/)