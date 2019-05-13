---
layout: post
title:  "Algorithm leetCode583 Delete Operation for Two Strings"
date:   2019-02-20
desc: "Algorithm leetCode583"
keywords: "Algorithm"
categories: [Cetus]
tags: [Algorithm]
icon: icon-html
---

最大公共子序列，我记得我会来着。。。我可能忘了怎么做了。。。

我寻思这个题一维也能做啊，空间可以优化一下，能超过百分之百的人

```c++
class Solution {
public:
    int minDistance(string word1, string word2) {
        int dp[505];
        memset(dp,0,sizeof(dp));
        int n=word1.size(),m=word2.size();
        int pre;
        for(int i=1;i<=n;i++){
            pre=0;
            for(int j=1;j<=m;j++)
            {
                int h=dp[j];
                dp[j]=max(dp[j],dp[j-1]);
                if(word1[i-1]==word2[j-1]){
                    dp[j]=pre+1;
                }
                pre=h;
            }
        }
        return m+n-2*dp[m];
    }
};
```

>CSAPP咕了那么久，算法就稍微写一下吧。。。