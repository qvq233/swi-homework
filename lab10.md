
# python编程实验报告

1. 计算极限$\lim\limits_{x \to 0} [\frac{cosx-1}{x-ln(1+x)}]$

```
limit((cos(x)-1)/(x-log(1+x)),x,0)
```

求arctanx在x=0点的局部泰勒公式

```
atan(x).series(x,0,6)
```

![](https://img-blog.csdnimg.cn/20190101004222176.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzM0NzA5NQ==,size_16,color_FFFFFF,t_70)

$\lim\limits_{x \to 0} [\frac{cosx-1}{x-ln(1+x)}]$ = -1




2. 求矩阵的逆
$$
 \left[
 \begin{matrix}
   1 & 0 & -2 \\
   -3 & 1 & 4 \\
   2 & -3 & 4
  \end{matrix}
  \right] \tag{3}
$$

```
a = mat([[1,0,-2],[-3,1,4],[2,-3,4]])
a.I
```

结果为$$
 \left[
 \begin{matrix}
   8 & 3 & 1 \\
   10 & 4 & 1 \\
   3.5 & 1.5 & 0.5
  \end{matrix}
  \right] \tag{3}
$$

矩阵乘法

$$
 \left[
 \begin{matrix}
   1 & 2 & 3 \\
   4 & 5 & 6 \\
   7 & 8 & 9
  \end{matrix}
  \right] \tag{3}
$$

$$
 \left[
 \begin{matrix}
   1 & 4 & 7 \\
   2 & 5 & 8 \\
   3 & 6 & 9
  \end{matrix}
  \right] \tag{3}
$$

```
A = mat([[1,2,3],[4,5,6],[7,8,9]])
B = mat([[1,4,7],[2,5,8],[3,6,9]])
A*B
```

结果为

$$
 \left[
 \begin{matrix}
   14 & 32 & 50 \\
   32 & 77 & 122 \\
   50 & 122 & 194
  \end{matrix}
  \right] \tag{3}
$$

![](https://img-blog.csdnimg.cn/2019010112581492.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzM0NzA5NQ==,size_16,color_FFFFFF,t_70)