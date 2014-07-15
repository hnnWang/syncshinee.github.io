---
layout: page
title: "Source of    B Prime Judge"
date: 2014-07-14 22:35
comments: true
sharing: true
footer: true
---
```c++
#include<cstdio>
#include<cmath>
using namespace std;

int main() {
	int a, b, count, factor1, factor2, sqrtc, sqrtf, sqrtn;
	bool flag1, flag2, flag;
	while (scanf("%d%d", &a, &b) != EOF) {
		count = a*a+b*b;
		sqrtc = (int) sqrt(count);
		flag = false;
		//特判: -1 -i 0 i 1 不是复素数
		if (count < 2) {
			flag = true;
		}
		//枚举第一个因数
		for (int i = 2; i <= sqrtc; ++i) {
			if (count % i == 0) {
				factor1 = i;
				factor2 = count / i;
				flag1 = false;
				flag2 = false;
				//枚举判断该因数是不是平方和数
				sqrtf = (int) sqrt(factor1);
				for (int j = 0; j <= sqrtf; ++j) {
					sqrtn = (int) sqrt(factor1-j*j);
					if (sqrtn*sqrtn == factor1-j*j) {
						flag1 = true;
						break;
					}
				}
				//若不是，则不必对第二个因数进行判断
				if (!flag1)
					continue;
				//枚举判断第二个因数是不是平方和数
				sqrtf = (int) sqrt(factor2);
				for (int j = 0; j <= sqrtf; ++j) {
					sqrtn = (int) sqrt(factor2-j*j);
					if (sqrtn*sqrtn == factor2-j*j) {
						flag2 = true;
						break;
					}
				}
				//如果两个因数都是,则该素数不是复素数
				if (flag1 && flag2) {
					flag = true;
					break;
				}
			}
		}
		if (flag)
			printf("NO\n");
		else
			printf("YES\n");
	}
	return 0;
}
```
[返回](/blog/2014/07/14/bupt0solution/)