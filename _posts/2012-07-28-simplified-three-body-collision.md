---
layout: post
date: '2012-07-28'
title: 关于三质点同时碰撞问题的一组特解
---

### 关于三质点同时碰撞问题的一组特解

## 摘要

在低速运动力学下，两已知质量与初速度的质点完全弹性碰撞后的动能，动量情况已经被完整的研究并且得到解释解。但是，三个已知质量与初速度的质点同时碰撞后的动能动量情况依然没被研究清楚。本论文提出一种解决思路并且给出一组简单情况下的特解。

## 背景

两已知质量与初速度的质点完全弹性碰撞的情况下，若已知以下数据：质量分别为 m1，m2，初速度 v1，v2，设碰撞后两质点速度 x，y，则根据动量守恒定律与能量守恒定律可列得方程组：

`m1 * v1 + m2 *v2 = m1 * x + m2 * y`  
`1/2 * m1 * v1 * v1 + 1/2 * m2 * v2 * v2 = 1/2 * m1 * x * x + 1/2 * m2 * y * y`


动量守恒式为矢量式。

这里只考虑在同一直线上运动的两个质点，则有 v1 > v2，使用 Mathematica 得

`Solve[{m1 v1 + m2 v2 == m1 x + m2 y, 1/2 m1 v1 v1 + 1/2 m2 v2 v2 == 1/2 m1 x x + 1/2 m2 y y}, {x, y}]`  
`{ {x->v1,y->v2},{x->(m1 v1-m2 v1+2 m2 v2)/(m1+m2),y->(2 m1 v1-m1 v2+m2 v2)/(m1+m2)} }`

也就是说，{ {x}, {y} } 相当于给向量 { {v1},{v2} } 左乘了一个矩阵：

`T = { {m1 - m2, 2 m2}, {2 m1, m2 - m1} }/(m1 + m2)`  
`{ {x}, {y} } = T.{ {v1}, {v2} }`

两质点碰撞问题用此方法得到完全解决。
 
## 问题

三个已知质量与初速度的质点完全弹性碰撞的情况极为复杂。这篇论文我们只考虑以下比较简单的情景：

三个质点，分别排布于 x 轴上，初始坐标分别为 -v1, -v2, -v3，初始速度分别为 v1, v2, v3，质量分别为 m1, m2, m3。有 v1 > v2 > v3。则有单位时间后它们于原点同时相撞。假设不考虑万有引力，产生的是完全弹性碰撞。求碰撞之后三质点分别速度 x, y, z。则根据动量守恒定律与动能守恒定律至少有以下两条式子成立：

`m1 * v1 + m2 * v2 + m3 * v3 = m1 * x + m2 * y + m3 * z`  
`1/2 * m1 * v1 * v1 + 1/2 * m2 * v2 * v2 + 1/2 * m3 * v3 * v3= 1/2 * m1 * x * y + 1/2 * m2 * y * y + 1/2 * m3 * z *z`

但是，方程组只有两个方程，三个未知数，数学解有无穷组。但实际情况上，我们相信 x, y, z 是确定的。

## 思路与一组特解

令三质点编号 P, Q, R。

这是一个分析思路。不妨假设相撞不是同时的，而是在很短的时间内 P 质点先与 Q 质点相撞，之后 Q 质点再和 R 质点相撞，之后 Q 质点可能再和 P 质点相撞......而这一切在很短时间内发生，直到 P 质点不能追上 Q 质点，Q 质点不能追上 R 质点，则它们此时的速度即为 x, y, z。这样的过程就化归为两球碰撞问题

但数学分析得到的结果显示解答并没有这么简单。

若这个思路是正确的话，则有另外一个思路，假设很短的时间内不是 P 和 Q 先相撞，而是 Q 和 R 先相撞，之后过程一样。两个思路是“对称”的得到的结果应该一样。

简单的数据模拟我们可以发现，（第一个思路中）碰撞过程最多只发生三次：PQ, QR, PQ，即达到碰撞终止条件。
令

`A = { {m1 - m2, 2 m2, 0}, {2 m1, m2 - m1, 0}, {0, 0, m1 + m2} }/(m1 + m2)`  
`B = { {m2 + m3, 0, 0}, {0, m2 - m3, 2 m3}, {0, 2 m2, m3 - m2} }/(m2 + m3)`

若很短时间内碰撞三次后三质点速度稳定下来，则两个思路得到的结果应该是一样的，满足

```
{ {x}, {y}, {z} } = A.B.A.{ {v1}, {v2}, {v3} } = B.A.B.{ {v1}, {v2}, {v3} }
```

用 Mathematica

`Solve[A.B.A == B.A.B, {m3}]`  
`{ {m3 -> (m2 (m1 + m2))/(3 m1 - m2)} }`


也就是说，只有 `m3 = (m2 (m1 + m2))/(3 m1 - m2)` ，才有可能满足这对思路，才能把直线上三质点碰撞问题模拟成两质点碰撞问题来解决。

此时

`FullSimplify[A.B.A]`  
`{ {-1 + (2 m1)/(m1 + m2), ((m1 - m2) m2)/(m1 (m1 + m2)), m2/m1}, {-1 + (2 m1)/(m1 + m2), 5/2 - m2/(2 m1) - (2 m1)/(m1 + m2), 1/2 (-1 + m2/m1)}, {-1 + (4 m1)/(m1 + m2), 5/2 - m2/(2 m1) - (4 m1)/(m1 + m2), 1/2 (-1 + m2/m1)} }`

## 实验

有待考察。

## 结论

对于问题，我们可以找到一组特解，只需要满足

`m3 = (m2 (m1 + m2))/(3 m1 - m2)`

则有

`{ {x}, {y}, {z} } = A.B.A.{ {v1}, {v2}, {v3} }`  
`{ {x}, {y}, {z} } = B.A.B.{ {v1}, {v2}, {v3} }`
