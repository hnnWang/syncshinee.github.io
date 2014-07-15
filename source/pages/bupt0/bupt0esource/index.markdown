---
layout: page
title: "Source of E 最小距离查询"
date: 2014-07-14 22:36
comments: true
sharing: true
footer: true
---
```c++
#include<cstdio>
#include<cstring>
#include<string>
using namespace std;

char s[100010], command[8], ch[8];
int front[100010][26];
int behind[100010][26];

int min(int a, int b) {
	if (a < b)
		return a;
	return b;
}

int main() {
	int T, len, m, que;
	scanf("%d", &T);
	while (T--) {
		scanf("%s", s);
		len = strlen(s);
		memset(front, -1, sizeof(front));
		memset(behind, -1, sizeof(behind));
		
		//预处理出每一个点的前面及后面所有字母第一次出现的位置
		for (int i = 1; i < len; ++i) {
			for (int j = 0; j < 26; ++j)
				if (front[i-1][j] > -1)
					front[i][j] = front[i-1][j];
			front[i][s[i-1]-'a'] = i-1;
		}
		for (int i = len-2; i >= 0; --i){
			for (int j = 0; j < 26; ++j)
				if (behind[i+1][j] > -1)
					behind[i][j] = behind[i+1][j];
			behind[i][s[i+1]-'a'] = i+1;
		}

		scanf("%d", &m);
		while (m--) {
			scanf("%s", command);
			//对添加字母的操作,维护数组
			if (strcmp(command,"INSERT") == 0) {
				scanf("%s", ch);
				len++;
				strcat(s, ch);
				for (int i = 0; i < 26; ++i)
					if (front[len-2][i] > -1)
						front[len-1][i] = front[len-2][i];
				front[len-1][s[len-2]-'a'] = len-2;
				int tmp = front[len-1][s[len-1]-'a'];
				for (int i = tmp; i < len-1; ++i)
					behind[i][s[len-1]-'a'] = len-1;
			}
			//对查询操作,分情况输出
			else {
				scanf("%d", &que);
				if (front[que][s[que]-'a'] == -1 && behind[que][s[que]-'a'] == -1)
					printf("-1\n");
				else if (front[que][s[que]-'a'] == -1)
					printf("%d\n", behind[que][s[que]-'a'] - que);
				else if (behind[que][s[que]-'a'] == -1)
					printf("%d\n", que-front[que][s[que]-'a']);
				else printf("%d\n", min(que-front[que][s[que]-'a'], behind[que][s[que]-'a']-que));
			}
		}

	}
	return 0;
}
```
[返回](/blog/2014/07/14/bupt0solution/)