
\> Problem: [59. 螺旋矩阵 II](https://leetcode.cn/problems/spiral-matrix-ii/description/)

[TOC]

# 思路

>  螺旋矩阵，具体是没有什么算法应用的，主要是考察对于循环的理解，所以核心问题是如何在循环中做到遍历二维数组的每一个点。

# 解题方法

> 首先要考虑循环的思路，因为是一个正方形矩阵，可以拆分成一层一层来理解，每一层有四条边，对四条边进行赋值操作。那么也就有了这些变量：1、round，赋值轮数，也就是正方形可以拆成几层；2、赋值区间，因为要四条变互不干涉，所以可以考虑都是使用左闭右开的区间；3、key，也就是要赋的值，从1自增到n*n；4、startx，starty开始位置，对每一轮的这个点进行赋值

> 首先对正方形的上面那条边进行赋值，假如n=3，那么就是赋值1,2，对于二维数组matrix[i][j],就是对j进行操作，在i=startx的情况下，j从1赋值到n-2（因为右边是开区间，所以不是n-1），此时又涉及到一个新的变量，第一轮赋值到n-2，第二轮就要n-3，以此类推…，所以还有设置一个offset=1，每一轮只需要赋值到n-offset即可。

> 然后对右边那条边赋值，也就是在n=3的情况下赋值到3,4，对于二维数组matrix[i][i],就是对i进行操作，在上一轮赋值时，j已经自增到了n-1，那么赋值是可以直接使用j，也就是matrix[i][j],i的范围是从starty开始，到n-offset结束。

> 随后赋值下面那条边，也就是5,6，在此时i已经自增到了n-1，此时对j进行修改即可，同样是matrix[i][j],j的范围就是n-offset开始，startx结束。

> 最后对左边进行赋值，同样的道理，修改i值即可，从n-offset开始，到starty结束。

四轮赋值结束后，因为开始的位置发生了变动，所以startx，starty的值都+1即可，因为要进行下一轮操作，所以round也+1，key则始终保持自增状态。

> 显然，这只是进行了一轮操作，那么只需要把这些操作放入一个while循环，就能做到一轮一轮的赋值，最后判断的条件就是轮数，显然轮数和n的关系是round=n/2

在n为奇数时，我们会发现正方形的中心有一个点无法赋值，只需要在循环外新增一个判定条件，当n为奇数时，矩阵中心单独赋值即可。

**# Code**

```java
class Solution {

​    public int[][] generateMatrix(int n) {

​    int[][] matrix= new int[n][n];

​        int startx=0;

​        int starty=0;

​        int round=1;

​        int key=1;

​        int i=0,j=0;

​        int offset=1;

​        if((n/2)*2 != n)

​        {

​            matrix[n/2][n/2]=n*n;

​        }

​        while(round<=n/2)

​        {

​            for(j=startx;j<n-offset;j++)

​            {

​                matrix[startx][j]=key;

​                key++;

​            }

​            for(i=starty;i<n-offset;i++)

​            {

​                matrix[i][j]=key;

​                key++;

​            }

​            for(j=n-offset;j>startx;j--)

​            {

​                matrix[i][j]=key;

​                key++;

​            }

​            for (i=n-offset;i>starty;i--)

​            {

​                matrix[i][j]=key;

​                key++;

​            }

​            startx++;

​            starty++;

​            round++;

​            offset++;

​        }

​        return matrix;

​    }

}
```
