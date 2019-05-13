---
layout: post
title:  "Algorithm leetCode43 Multiply Strings"
date:   2019-02-20
desc: "Algorithm-leetCode43"
keywords: "Algorithm"
categories: [Cetus]
tags: [Algorithm]
icon: icon-html
---


>大数乘法板子题，但是这个鬼畜的oj让我一脸懵逼，只能写其中一个函数，返回一个字符串，输出的字符串才是最终答案。。。
>
>所以最后答案数组还要转换成string类，我直接一个一个相等还会RE。。。只能先转换成字符数组，再转换成string，鬼畜得一匹

```c++
class Solution {
public:
    string multiply(string num1, string num2) {
        int a[110];
        int b[110];
        int c[220];
        memset(a,0,sizeof(a));
        memset(b,0,sizeof(b));
        memset(c,0,sizeof(c));
        int len1=num1.size();
        int len2=num2.size();
        for(int i=0;i<len1;i++)
            a[len1-i]=num1[i]-'0';
        for(int i=0;i<len2;i++)
            b[len2-i]=num2[i]-'0';
        if(len1==1&&num1[0]=='0'){
            string ans;ans[0]='0';
            return ans;
        }
        if(len2==1&&num2[0]=='0'){
            string ans;ans[0]='0';
            return ans;
        }
        int m=0;
        for(int i=1;i<=len1;i++)
            for(int j=1;j<=len2;j++)
            {
                c[i+j-1]+=a[i]*b[j];
                m=max(m,i+j-1);
            }
        for(int i=2;i<=m;i++)
        {
            c[i]+=c[i-1]/10;
            c[i-1]%=10;
        }
        while(c[m]/10){
            c[m+1]+=c[m]/10;
            c[m]%=10;
            m++;
        }
        char s[220];
        memset(s,0,sizeof(s));
        for(int i=1;i<=m;i++)
            s[i-1]=c[m-i+1]+'0';
        string ans(s);
        return ans;
    }
};
```