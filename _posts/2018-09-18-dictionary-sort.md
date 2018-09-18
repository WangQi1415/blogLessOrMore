---
layout: post
title:  "字典序法"
date:   2018-09-12 14:30:01 +0800
categories: 算法之旅
tag: Algorithm
---

* content
{:toc}


前两天上组合数学课，学到了字典序法，今天上机实现下。
### 问题描述
给定一个全排列你，使用字典序法求出下一个排列，即这个排列与下一个排列之间没有其他的排列。就要求这一个与下一个有尽可能长的共同前缀，也即变化限制在尽可能短的后缀上。
例如：  
1&nbsp;&nbsp;2&nbsp;&nbsp;3&nbsp;&nbsp;4  
1&nbsp;&nbsp;2&nbsp;&nbsp;4&nbsp;&nbsp;3  
1&nbsp;&nbsp;3&nbsp;&nbsp;2&nbsp;&nbsp;4  
...  
...  
...  
4&nbsp;&nbsp;3&nbsp;&nbsp;2&nbsp;&nbsp;1  

### 解决方案
按字典排列的生成算法，可归纳其规律为：  
S1：求满足关系式 Pj-1< Pj的j的最大值，设为i,即  
&nbsp;&nbsp;&nbsp;&nbsp;i=max{j|Pj-1<Pj}  
(说通俗点，即从右往左找，找第一个左边比相邻右边大的数字)
  
 S2: 求满足关系式 Pi-1< Pk的k的最大值，设为h,即  
 &nbsp;&nbsp;&nbsp;&nbsp;h=max{k|Pi-1<Pk} 
(从右往左找找第一个比i-1位置大的数)

S3: 互换 Pi-1与Pk  
S4:将Pi-1后的子序列逆序，便得到下一排列  

### 代码实现
```
#include<iostream>
using namespace std;

int capacity;
int size;//记录数组的下标
int* add(int* a,int x){//指针作为形参来传递，可以改变指针指向的形参的内容，但是无法改变原来的指针
	if(size==capacity){
		int* b=new int[capacity+5];
		capacity+=5;
		for(int i=0;i<size;i++){
			b[i]=a[i];
		}
		a=b;
	}
	a[size]=x;
	size++;
	return a;
}
void swap(int &x,int &y){
	int tmp;
	tmp=x;
	x=y;
	y=tmp;
}
void view(int* a){
	for(int i=0;i<size;i++)
	{
		cout<<a[i]<<" ";
	}
	cout<<endl;
}
void inverse(int* a, int fromindex){
	for(int i=fromindex,j=size-1;i<j;i++,j--){
		swap(a[i],a[j]);
	}
}
bool permutation(int* a){
	int j,k;
	bool flag=false;//标记是否为最后一个序列
    for(j=size-1;j>0;j--){
		if(a[j-1]<a[j]){
			flag=true;
			break;
		}
	}
	if(!flag){
		cout<<"排列完成";
			return false;
	}
	for(k=size-1;k>=j;k--){
		if(a[k]>a[j-1])
			break;
	}
	//cout<<a[k]<<" "<<a[j-1]<<endl;
	swap(a[k],a[j-1]);
	inverse(a,j);
//	cout<<a[k]<<" "<<a[j-1]<<endl;
	return true;
}
int main(){
	int* a=new int[10];
	capacity=10;
	size=0;
	int x;
	while(cin>>x){
		a=add(a,x);
		char ch=getchar();
		if(ch=='\n')
			break;
	}
	
	while(permutation(a)){
		view(a);
	}
	
	
}
```


