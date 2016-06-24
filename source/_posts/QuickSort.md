---
title: QuickSort
tags:
  - Algorithm
  - C#
categories: Code
date: 2015-01-16
---


### 快速排序

[Wiki](http://zh.wikipedia.org/wiki/%E5%BF%AB%E9%80%9F%E6%8E%92%E5%BA%8F)

> 快速排序是由东尼·霍尔所发展的一种排序算法。在平均状况下，排序 n 个项目要Ο(n log n)次比较。在最坏状况下则需要Ο(n2)次比较，但这种状况并不常见。事实上，快速排序通常明显比其他Ο(n log n) 算法更快，因为它的内部循环（inner loop）可以在大部分的架构上很有效率地被实现出来。

+ 算法

> 快速排序使用分治法（Divide and conquer）策略来把一个序列（list）分为两个子序列（sub-lists）。
> 步骤为：

1. 从数列中挑出一个元素，称为 "基准"（pivot）

2. 重新排序数列，所有元素比基准值小的摆放在基准前面，所有元素比基准值大的摆在基准的后面（相同的数可以到任一边）。在这个分区退出之后，该基准就处于数列的中间位置。这个称为分区（partition）操作。

3. 递归地（recursive）把小于基准值元素的子数列和大于基准值元素的子数列排序。

<!-- more -->

+ Code

<hr/>

``` csharp
    class QuickSort{    
        static void main(string[] args){
            //声明数组
            int[] arr={2,0,5,9,7,6,3};
            //调用排序方法
            Sort(arr,0,arr.Lenthg-1);
            Console.ReadKey();
        }
        /// <summary>
        /// 快速排序
        /// </summary>
        /// <param name="arr">数组</param>
        static void Sort(int[] arr,int low,int high)
        {
            //获取数组中间数
            int middle = arr[(low+high)/2];
            //赋值
            int i = low, j= high;
            //输出i,j,middle
            Console.WriteLine(i + "\t" + j+"\t"+middle);
            while (i <= j)
            {
                while (arr[i] < middle&&i<high) i++;
                while (arr[j] > middle&&j>low) j--;
                if (i <= j)
                {
                    int temp = arr[i];
                    arr[i] = arr[j];
                    arr[j] = temp;
                    i++;
                    j--;
                }
            }
            //输出数组
            Console.WriteLine(string.Join(",", arr));
            //尾递归
            if(j>low) Sort(arr,low,j);
            if(i<high) Sort(arr, i, high);
        }
    }
```
