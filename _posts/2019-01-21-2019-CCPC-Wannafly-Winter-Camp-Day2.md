---
layout: post
title:  "2019 CCPC Wannafly Winter Camp Day2"
date:   2019-01-21
desc: "2019 CCPC Wannafly Winter Camp"
keywords: "Camp"
categories: [Acm]
tags: [2019 CCPC Wannafly Winter Camp]
icon: icon-html
---

_____

今天，我又自闭了，A题wa了三页，只是因为一点小错误，我知道我还有很多不足，继续努力吧。

## A Erase Numbers Ⅱ

>大坑啊，这道题极限数据会爆 long long，比如 n = 2 , a[1] = 1e9 , a[2] = 1e9，我承认我还是太年轻，其实稍微去试一下，就不会wa一下午。

这道题其实真的满水的，找到最大值maxx，和最大值左侧的最大数maxx1，以及最大值右侧的maxx2，然后比较maxx1 maxx和maxx maxx2哪个大就行了。
```c++
#include<iostream>
#include<cstdio>
#include<cmath>
#include<cstring>
#include<algorithm>
#include<string>
#define ll unsigned long long
using namespace std;
const int N=6005;
ll v[20];
ll a[N];
int get(ll x)
{
	int ans=0;
	while(x){
		ans++;
		x/=10;
	}
	return ans;
}
int main()
{
	int t;
	cin>>t;
	//ios::sync_with_stdio(false);
	int x=0;
	v[1]=10;
	for(int i=2;i<=11;i++)
	{
		v[i]=v[i-1]*10;
	}
	while(t--)
	{
		x++;
		int n;
		memset(a,0,sizeof(a));
		cin>>n;
		for(int i=1;i<=n;i++)cin>>a[i];
		cout<<"Case #"<<x<<": ";
		ll maxx=0;
		int p;
		for(int i=1;i<=n;i++)
		{
			if(a[i]>=maxx){
				maxx=a[i];
				p=i;
			}
		}
		ll maxx1=0,maxx2=0;
		for(int i=p-1;i>=1;i--)
		{
			if(a[i]>maxx1){
				maxx1=a[i];
			}
		}
		for(int i=p+1;i<=n;i++)
		{
			if(a[i]>maxx2){
				maxx2=a[i];
			}
		}
		ll ans1,ans2;
		int h1=get(maxx);	//这里不能用log10，会RE，但是我不知道为什么
		int h2=get(maxx2);
		ans1=maxx+maxx1*v[h1];
		ans2=maxx2+maxx*v[h2];
		ll ans;
		if(ans1>ans2)ans=ans1;
		else ans=ans2;
		cout<<ans<<endl;
	}
}
```
## B Erase Numbers I

	unsolved

## C Fibonacci Strikes Back

	unsolved

## D Honeycomb

	unsolved

## E Power of Function

	unsolved

## F Quicksort

	unsolved

## G Linear Congruential Generator

	unsolved

## H Honeycomb

球缺的体积公式为
$$ V=\pi H^2 (R-\frac{H}{3}) $$
推导很简单，就是一个不完整的圆绕y轴旋转所成的体积，从 R - H 到 R 对 y 积分求体积，整理一下就可以了。

知道该如何求球缺体积后，对于每课行星，只需要求一下行星和大球的球缺的体积，再把他们减去就可以了。

	unsolved

## I Square Subsequences

	unsolved

## J Square Substrings

	unsolved

## K Sticks

	unsolved
	
## L Pyramid

	unsolved
	
	
	

