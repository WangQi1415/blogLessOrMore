---
layout: post
title:  "Day01-POJ3299-Humidex"
date:   2018-08-03 14:37:01 +0800
categories: 算法之旅
tag: Algorithm
---

* content
{:toc}


###[Humidex](http://poj.org/problem?id=3299) 
####题目描述
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Ancient Roman empire had a strong government system with various departments, including a secret service department. Important documents were sent between provinces and the capital in encrypted form to prevent eavesdropping. The most popular ciphers in those times were so called substitution cipher and permutation cipher. 
Substitution cipher changes all occurrences of each letter to some other letter. Substitutes for all letters must be different. For some letters substitute letter may coincide with the original letter. For example, applying substitution cipher that changes all letters from 'A' to 'Y' to the next ones in the alphabet, and changes 'Z' to 'A', to the message "VICTORIOUS" one gets the message "WJDUPSJPVT". 
Permutation cipher applies some permutation to the letters of the message. For example, applying the permutation <2, 1, 5, 4, 3, 7, 6, 10, 9, 8> to the message "VICTORIOUS" one gets the message "IVOTCIRSUO". 
It was quickly noticed that being applied separately, both substitution cipher and permutation cipher were rather weak. But when being combined, they were strong enough for those times. Thus, the most important messages were first encrypted using substitution cipher, and then the result was encrypted using permutation cipher. Encrypting the message "VICTORIOUS" with the combination of the ciphers described above one gets the message "JWPUDJSTVP". 
Archeologists have recently found the message engraved on a stone plate. At the first glance it seemed completely meaningless, so it was suggested that the message was encrypted with some substitution and permutation ciphers. They have conjectured the possible text of the original message that was encrypted, and now they want to check their conjecture. They need a computer program to do it, so you have to write one.

<b>Input</b>  

Input contains two lines. The first line contains the message engraved on the plate. Before encrypting, all spaces and punctuation marks were removed, so the encrypted message contains only capital letters of the English alphabet. The second line contains the original message that is conjectured to be encrypted in the message on the first line. It also contains only capital letters of the English alphabet. 
The lengths of both lines of the input are equal and do not exceed 100.

<b>Output</b>

Output "YES" if the message on the first line of the input file could be the result of encrypting the message on the second line, or "NO" in the other case.
生词：  
in encrypted 加密的
eavesdropping 窃听
ciphers 密码 


```
#include<iostream>
#include<cmath>
#include<iomanip>
using namespace std;


/*
比较坑的是这道题的输入是  可以输入 T H D 中的任意两个
*/
int main(){
	char ch1,ch2;
	double T,D,H;
	double i,exp,h,e;
	while(cin>>ch1){
		if(ch1=='E'){
			break;
		}else{
			switch(ch1){
				case'T':
					cin>>T;
					cin>>ch2;
					switch(ch2){
						case'D':
							cin>>D;
							i=5417.7530*((1/273.16)-(1/(D+273.16)));
							exp=pow(2.718281828,i);
							e=6.11*exp;
							h=(0.5555)*(e-10.0);
							H=T+h;	
							break;
						case'H':
							cin>>H;
							h=H-T;
							e=h/0.5555+10.0;
							exp=e/6.11;
							i=log(exp);
							D=1/(1/273.16-i/5417.7530)-273.16;
							break;
									
					}
					break;
				case'H':
					cin>>H;
					cin>>ch2;
					switch(ch2){
						case'T':
							cin>>T;
							h=H-T;
							e=h/0.5555+10.0;
							exp=e/6.11;
							i=log(exp);
							D=1/(1/273.16-i/5417.7530)-273.16;
							break;	
						case'D':
							cin>>D;
							i=5417.7530*((1/273.16)-(1/(D+273.16)));
							exp=pow(2.718281828,i);
							e=6.11*exp;
							h=(0.5555)*(e-10.0);
							T=H-h;
							break;
					}
					break;
				case'D':
					cin>>D;
					cin>>ch2;
					switch(ch2){
						case'H':
							cin>>H;
							i=5417.7530*((1/273.16)-(1/(D+273.16)));
							exp=pow(2.718281828,i);
							e=6.11*exp;
							h=(0.5555)*(e-10.0);
							T=H-h;
							break;
						case'T':
							cin>>T;
							i=5417.7530*((1/273.16)-(1/(D+273.16)));
							exp=pow(2.718281828,i);
							e=6.11*exp;
							h=(0.5555)*(e-10.0);
							H=T+h;	
							break;
					}
			}
				
			cout.setf(ios::fixed);
			cout.precision(1);
 			cout<<'T'<<" "<<T<<" "<<'D'<<" "<<D<<" "<<'H'<<" "<<H<<endl;
		}
	}
	
	return 0;
}

```