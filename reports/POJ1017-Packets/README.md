## [[POJ](http://poj.org/)] [[INDEX](https://github.com/lyy289065406/POJ-Solving-Reports)] [1017] [[Packets](http://poj.org/problem?id=1017)]

> [Time: 1000MS] [Memory: 10000K] [难度: 初级] [分类: 逻辑推理]

------

## 问题描述

一个工厂制造的产品形状都是长方体盒子，它们的高度都是 h，长和宽都相等，一共有六个型号，分别为 `1*1`, `2*2`, `3*3`, `4*4`, `5*5`, `6*6`。

这些产品通常使用一个 `6*6*h` 的长方体箱子包装然后邮寄给客户。因为邮费很贵，所以工厂要想方设法的减小每个订单运送时的箱子数量BoxNum。


## 解题思路

**逻辑推理**题。

由于盒子和箱子的高均为h，因此只需考虑底面积的空间。

- `6*6` 的盒子，每个盒子独占一个箱子。
- `5*5` 的盒子，每个盒子放入一个箱子，该箱子的剩余空间允许放入的最大尺寸为 `1*1` ，且最多放11个。
- `4*4` 的盒子，每个盒子放入一个箱子，该箱子的剩余空间允许放入的最大尺寸为 `2*2` 。
- `3*3` 的盒子，每4个刚好独占一个箱子，不足4个 `3*3` 的，剩下空间由 `2*2` 和 `1*2` 填充。
- `2*2` 的盒子和 `1*1` 的盒子主要用于填充其他箱子的剩余空间，填充后的多余部分才开辟新箱子装填。


详细解题思路看程序注释。


## AC 源码


```c
//Memory Time 
//248K   32MS 

#include<iostream>
using namespace std;

int max(int a,int b)
{
	return a>b?a:b;
}

int main(void)
{
	int s1,s2,s3,s4,s5,s6;      //6种size的盒子数量
	while(cin>>s1>>s2>>s3>>s4>>s5>>s6 && (s1+s2+s3+s4+s5+s6))
	{
		int BoxNum=0;           //放进所有盒子所需的最少箱子数

		BoxNum+=s6;             //6*6的盒子，每个都刚好独占一个箱子

		BoxNum+=s5;             //5*5的盒子，放进箱子后，每个箱子余下的空间只能放11个1*1的盒子
		s1=max(0,s1-s5*11);     //把1*1的盒子尽可能地放进已放有一个5*5盒子的箱子

		BoxNum+=s4;             //4*4的盒子，放进箱子后，每个箱子余下的空间为5个2*2的盒子空间
		                        //先把所有2*2的盒子尽可能地放进这些空间
		if(s2>=s4*5)             //若2*2的盒子数比空间多
			s2-=s4*5;           //则消去已放进空间的部分
		else                    //若2*2的盒子数比空间少
		{                       //则先把所有2*2的盒子放进这些空间
			s1=max(0,s1-4*(s4*5-s2));   //再用1*1的盒子填充本应放2*2盒子的空间
			s2=0;               //一个2*2空间可放4个1*1盒子
		}

		BoxNum+=(s3+3)/4;       //每4个3*3的盒子完全独占一个箱子
		s3%=4;            //3*3的盒子不足4个时，都放入一个箱子，剩余空间先放2*2，再放1*1
		if(s3)
		{                       //当箱子放了i个3*3盒子，剩下的空间最多放j个2*2盒子
			if(s2>=7-2*s3)       //其中i={1,2,3} ; j={5,3,1}  由此可得到条件的关系式
			{
				s2-=7-2*s3;
				s1=max(0,s1-(8-s3));  //当箱子放了i个3*3盒子，并尽可能多地放了个2*2盒子后
			}                         //剩下的空间最多放j个1*1盒子，其中i={1,2,3} ; j={7,6,5}
			else                //但当2*2的盒子数不足时，尽可能把1*1盒子放入剩余空间
			{  //一个箱子最多放36个1*1，一个3*3盒子空间最多放9个1*1，一个2*2盒子空间最多放4个1*1
				s1=max(0,s1-(36-9*s3-4*s2));    //由此很容易推出剩余空间能放多少个1*1
				s2=0;
			}
		}

		BoxNum+=(s2+8)/9;       //每9个2*2的盒子完全独占一个箱子
		s2%=9;            //2*2的盒子不足9个时，都放入一个箱子，剩余空间全放1*1
		if(s2)
			s1=max(0,s1-(36-4*s2));

		BoxNum+=(s1+35)/36;     //每36个1*1的盒子完全独占一个箱子

		cout<<BoxNum<<endl;
	}
	return 0;
}
```

------

## 版权声明

　[![Copyright (C) EXP,2016](https://img.shields.io/badge/Copyright%20(C)-EXP%202016-blue.svg)](http://exp-blog.com)　[![License: GPL v3](https://img.shields.io/badge/License-GPL%20v3-blue.svg)](https://www.gnu.org/licenses/gpl-3.0)
  

- Site: [http://exp-blog.com](http://exp-blog.com) 
- Mail: <a href="mailto:289065406@qq.com?subject=[EXP's Github]%20Your%20Question%20（请写下您的疑问）&amp;body=What%20can%20I%20help%20you?%20（需要我提供什么帮助吗？）">289065406@qq.com</a>


------
