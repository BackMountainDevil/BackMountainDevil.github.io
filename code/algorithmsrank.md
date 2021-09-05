---
title: "Algorithmsrank"
date: 2021-03-13T15:39:32+08:00
lastmod: 2021-03-13T15:39:32+08:00
keywords: ['等值演算', '排名预测']
description: "用编程循环解决等值演算问题-竞赛排名预测对半问题"
tags: ['c', 'math']
categories: ['Algorithms']
author: "筱氚"
---
# 大一期末循环必考

反正当时我很蒙蔽

## 题目

5名跳水高手参加10m高台跳水决赛，有好事者让5人依据实力预测比赛结果。
A选手说：B第二，我第三。
B选手说：我第二，E第四。
C选手说：我第一，D第二。
D选手说：C最后，我第三。
E选手说：我第四，A第一。
决赛成绩公布后，每位选手的预测都只说对了一半，即一对一错。请编程解出比赛的实际名次。


## 解答

用continue跳过一些循环和错误解，相乘的时间会长一丢丢（计组原理）

```bash
#include <stdio.h>

int main()
{
	int a,b,c,d,e;
	for (a=1;a<6;a++){
		for (b=1;b<6;b++){
            if(a==b) continue;
			for (c=1;c<6;c++){
                if(a==c||b==c) continue;
				for (d=1;d<6;d++){
                    if(a==d||b==d||c==d) continue;
                    e=15-a-b-c-d;
                    if((b==2&&a!=3||b!=2&&a==3)&& 
                    (b==2&&e!=4||b!=2&&e==4)&& 
                    (c==1&&d!=2||c!=1&&d==2)&& 
                    (c==5&&d!=3||c!=5&&d==3)&& 
                    (e==4&&a!=1||e!=4&&a==1)){
                        printf("a=%d b=%d c=%d d=%d e=%d\n" ,a,b,c,d,e);
                    }
				}
			}
		}
	}
    return 0;
} 

```

# 等值演算

离散数学的坑，第五版耿同志的例题1.11 - p11  
ABCD四人进行百米竞赛，观众甲乙丙对成绩进行预测：  
甲说 C 第一名， B 第二名  
乙说 C 第二名，D 第三名  
丙说 A 第二名，D 第四名。  
结果大家都说对了一半，那么正确的名次（无并列名次）是怎样？ 

## 等值演算解法

尊重版权，看书qu吧（太长不想打字了。。。），结果是CADB

## 编程解法

老师要求用编程解决等值演算问题的时候。emm观众蒙蔽了。。。然后老师说这多简单啊。。大一就说过类似的了，换个皮肤就不认识了吗。。。。。类似上面的跳水排名预测问题。。。。

```bash
#include <stdio.h>

int main()
{
	int a,b,c,d;
	for (a=1;a<5;a++){
		for (b=1;b<5;b++){
            if(a==b) continue;
			for (c=1;c<5;c++){
                if(a==c||b==c) continue;
                d = 10-a-b-c;
                if((c==1&&b!=2 || c!=1&&b==2)&& 
                ( c==2&&d!=3 || c!=2&&d==3)&& 
                ( a==2&&d!=4 || a!=2&&d==4)){
                    printf("a=%d b=%d c=%d d=%d \n" ,a,b,c,d);
                }
			}
		}
	}
    return 0;
} 
```

# 参考

 前两个五重循环遍历全部结果 5^5,第三个四种循环！
 
[5名跳水高手参加跳水决赛排名 默宇宇 2015-03-11](https://blog.csdn.net/shangfeiyu199539/article/details/44202099): 0.422 s true隐式转换为1。不如直接` if((rankA&&rankB&&rankC&&rankD&&rankE )&&。。。`
>rankA=(((B==2)&&(A!=3)) || ((B!=2)&&(A==3)));
                        rankB=(((B==2)&&(E!=4)) || ((B!=2)&&(E==4)));
                        rankC=(((C==1)&&(D!=2)) || ((C!=1)&&(D==2))); 
                        rankD=(((C==5)&&(D!=3)) || ((C!=5)&&(D==3)));
                        rankE=(((E==4)&&(A!=1)) || ((E!=4)&&(A==1)));
                        if((rankA+rankB+rankC+rankD+rankE==5)&&A!=B&&A!=C&&A!=D&&A!=E&&
                        B!=C&&B!=D&&B!=E&&C!=D&&C!=E&&D!=E)

[5位运动员参加了10米台跳水比赛，有人让他们预测比赛结果 A选手说：B第二，我第三； B选手说：我第二，E第四； C选手说：我第一，D第二； D选手说：C最后，我第三； E选手说：我第四，A aviciiiii 2018-12-03](https://blog.csdn.net/aviciiiii/article/details/84781182):0.043s,判断妙啊
>if((b==2&&a!=3||b!=2&&a==3)&& 
						(b==2&&e!=4||b!=2&&e==4)&& 
						(c==1&&d!=2||c!=1&&d==2)&& 
						(c==5&&d!=3||c!=5&&d==3)&& 
						(e==4&&a!=1||e!=4&&a==1)&&
						(a*b*c*d*e==120))

[5名跳水高手参加10米高台跳水决赛，有人让5人根据实力预测比赛结果编程解出比赛结果的实际名次 Walden_tinghou 2013-06-02](https://blog.csdn.net/sunnyboy9/article/details/9007345):0.359s,编译提示少个`#include <cstring>`.逻辑还没看懂。。
continue用来减少循环次数
>if(a==d||b==d||c==d) continue;
       e=15-a-b-c-d;
       as=(b==2)+(a==3);
       bs=(b==2)+(e==4);
       cs=(c==1)+(d==2);
       ds=(c==5)+(d==3);
       es=(e==4)+(a==1);
       if(as==1&&bs==1&&cs==1&&ds==1&&es==1)

[使用逻辑等值演算简化代码   umisky lv-1 2017年12月25日](https://juejin.cn/post/6844903540838645767)：没看懂
