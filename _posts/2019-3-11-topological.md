---
layout:     post
title:      拓扑排序
subtitle:   有向无圈图的一种排序方法
date:       2019-3-11
author:     jxb2018
header-img: img/home-bg.jpg
catalog: 	 true
tags:
    - 图论
---
>   拓扑排序(Topological sorting)是对于有向无圈图顶点的一种排序

## 说明
- 它如果存在一条从vi到vj的路径，那么在排序中vj出现在vi的后面
- 如果图中含有圈，那么拓扑排序是不可能的

![image](https://jxb2018.github.io/img/toplogical_sort/1.jpg)

## 算法
- 先找出任意一个没有入边的顶点
- 显示该顶点，并将它和它的边一起从图中删除
- 重复上述操作1、2

## 过程详解
### 声明结构体
![image](https://jxb2018.github.io/img/toplogical_sort/2.jpg)
### 创建邻接表 //create_example_lgraph
#### step1 用一维数组_vexs存储顶点信息，用二维数组edges存储关联顶点信息
![image](https://jxb2018.github.io/img/toplogical_sort/3.jpg)
#### step2 用malloc函数分配指定字节大小的空间，将地址赋给pG、pG->vexs
![image](https://jxb2018.github.io/img/toplogical_sort/4.jpg)
#### step3 写入_vexs中的信息
![image](https://jxb2018.github.io/img/toplogical_sort/5.jpg)
#### step4 写入edges中的信息
以i=0时为例
a）创建VNode类型的对象，用进行赋值
![image](https://jxb2018.github.io/img/toplogical_sort/6.jpg)
b）将赋值好的对象，连接在对应的位置
![image](https://jxb2018.github.io/img/toplogical_sort/7.jpg)
最终得到邻接表:
![image](https://jxb2018.github.io/img/toplogical_sort/8.jpg)
#### step5 返回地址 pG

### 拓扑排序 //topological_sorting()
#### step1 创建ins(入度数组)、queue(辅助队列)、tops(拓扑排序结果数组)
![image](https://jxb2018.github.io/img/toplogical_sort/9.jpg)
#### step2 入度数组赋值
以i=0为例:
一开始，ins被memset函数初始化为零
![image](https://jxb2018.github.io/img/toplogical_sort/10.jpg)
声明ENode类型的指针node，并令其指向pG->vexs[0].first_edge
![image](https://jxb2018.github.io/img/toplogical_sort/11.jpg)
我们不妨将其单独拿出来讨论：
![image](https://jxb2018.github.io/img/toplogical_sort/12.jpg)
将node依次指向这条链上的所有结点、并将结点上ivex的值提取，在对应的ins数组上入度+1
i=0,完成后，得到如下的ins:
![image](https://jxb2018.github.io/img/toplogical_sort/13.jpg)
循环结束后，最终得到的ins：  
![image](https://jxb2018.github.io/img/toplogical_sort/14.jpg)
#### step3 将入度为零的顶点对应的序号，放入队中queue
![image](https://jxb2018.github.io/img/toplogical_sort/15.jpg)
#### step4 将入度为零的顶点，写入tops，并将相应的顶点入度减一
将入度为零的顶点写入tops
![image](https://jxb2018.github.io/img/toplogical_sort/16.jpg)
用node指向对应的顶点所在的链
![image](https://jxb2018.github.io/img/toplogical_sort/17.jpg)
用node一次指向链上的所有结点、并提取链上结点的ivex值，在ins的对应位置减1
![image](https://jxb2018.github.io/img/toplogical_sort/18.jpg)
#### step5 重复3、4直到没有顶点可以被写入tops
最终得到:
![image](https://jxb2018.github.io/img/toplogical_sort/19.jpg)
#### step6 判断tops中的元素的个数是否与图中顶点的数目相等，来判断是否图中含圈
#### step7 如果是有向无圈图，则输出拓扑排序结果