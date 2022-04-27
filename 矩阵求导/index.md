# 矩阵求导

# 矩阵求导
## 分子布局和分母布局
标量，向量，矩阵之间的求导，相对于标量对标量的求导，需要考虑一个额外的因素，就是求导之后的布局，例如标量对列向量求导之后是按照列向量排列还是按照行向量排列并没有一个确切的规定。因此这里我们引入分子布局和分母布局的概念。
* **分子布局**指的是求导之后的**排列方式和维度以分子为主**
  * 例如对于**列向量**$\mathbf{y}$而言，$\frac{\partial \mathbf{y}}{x}$，按照分子布局，得到的**结果也是列向量**。
  * 一个标量$y$对 **$m\times n$维的矩阵** $X$求导，其**结果为$n\times m$维**。
  * 一个$m$维的列向量$\mathbf{y}$对一个$n$维的行向量$\mathbf{x}$求导，其结果为
  <div>
  $$
  \frac{\partial \mathbf{y}}{\partial \mathbf{x}}=
  \begin{Bmatrix}
  \frac{\partial y_1}{\partial x_1} & 
  \frac{\partial y_1}{\partial x_2} &
  \cdots &
  \frac{\partial y_1}{\partial x_n}\\
  \frac{\partial y_2}{\partial x_1} &
  \frac{\partial y_2}{\partial x_2} &
  \cdots & 
  \frac{\partial y_2}{\partial x_n}\\
  \vdots & 
  \vdots & 
  \ddots & 
  \vdots \\
  \frac{\partial y_m}{\partial x_1} &
  \frac{\partial y_m}{\partial x_2} &
  \cdots & 
  \frac{\partial y_m}{\partial x_n}\\
  \end{Bmatrix}
  $$
  <div\>

  是一个$m \times n$维的矩阵，也被叫做**雅可比矩阵**
* 分母布局指的是求导之后的排列方式和维度以分母为主
  * 对于**列向量**$\mathbf{y}$而言，$\frac{\partial \mathbf{y}}{x}$，按照分母布局，得到的结果是**行向量**
  * 一个标量$y$对 **$m\times n$维的矩阵**$X$求导，其**结果为$m\times n$维**。
  * 一个$m$维的列向量$\mathbf{y}$对一个$n$维的行向量$\mathbf{x}$求导，其结果为
  <div>
  $$
  \frac{\partial \mathbf{y}}{\partial \mathbf{x}}=
  \begin{Bmatrix}
  \frac{\partial y_1}{\partial x_1} & 
  \frac{\partial y_2}{\partial x_1} &
  \cdots &
  \frac{\partial y_m}{\partial x_1}\\
  \frac{\partial y_1}{\partial x_2} &
  \frac{\partial y_2}{\partial x_2} &
  \cdots & 
  \frac{\partial y_m}{\partial x_2}\\
  \vdots & 
  \vdots & 
  \ddots & 
  \vdots \\
  \frac{\partial y_1}{\partial x_n} &
  \frac{\partial y_2}{\partial x_n} &
  \cdots & 
  \frac{\partial y_m}{\partial x_n}\\
  \end{Bmatrix}
  $$
  <div\>

  是一个$n \times m$维的矩阵，也被叫做**梯度矩阵**

  因此对于标量，向量和矩阵的求导可按下述表格来定义：

|  自变量/因变量   | 标量$y$  | $m$维列向量$\mathbf{y}$ | $m \times n$维矩阵$Y$|
| :----: | :----: | :----: | :----: |
| **标量$x$**  | / | $\frac{\partial \mathbf{y}}{\partial x}$<br>分子布局：$m$维列向量<br>分母布局：$m$维行向量| 分子布局：$m \times n$维矩阵<br>分母布局：$n \times m$维矩阵|
| **$m$维列向量$\mathbf{x}$**  | $\frac{\partial y}{\partial \mathbf{x}}$<br>分子布局：$m$维行向量<br>分母布局：$m$维列向量 |$\frac{\partial \mathbf{y}}{\partial \mathbf{x}}$<br>分子布局：$m \times n$维矩阵<br>分母布局：$n \times m$维矩阵 | / |
|**$m \times n$维矩阵$X$** | $\frac{\partial y}{X}$<br>分子布局：$n \times m$维矩阵<br>分母布局：$m \times n$维矩阵 | / | / |

## 求导方法--微分法
> 本节讨论的内容基于分母布局
### 标量和向量的微分和导数
在微积分中对于标量的导数和微分有如下公式
$$ df=f^\prime dx $$
多变量的情况下，则有如下公式（其中$\mathbf{x}$为列向量）:
$$df=\sum\limits_{i=1}^n\frac{\partial f}{\partial x_i}dx_i = (\frac{\partial f}{\partial \mathbf{x}})^Td\mathbf{x}$$
多变量的情况其实也是标量对向量的微分
### 矩阵的微分和导数
上述标量的微分和导数同样可以扩展到矩阵上，对于矩阵微分，有如下定义（其中$X$为矩阵，$X_{i,j}$为矩阵中位置为$i,j$的值）:
$$df = \sum\limits_{i=1}^m \sum\limits_{j=1}^n\frac{\partial f}{\partial X_{i,j}}dX_{i,j} = tr((\frac{\partial f}{\partial X})^TdX)$$
> 因为矩阵的乘法是前一个矩阵的行和后一个矩阵的列逐项相乘，对偏导加转置，便于我们在最后矩阵的基础上找到一组较为方便的元素来得到最后的全微分。
>
> 同时我们只需要对应的偏导数和对应的元素的微分的乘积，所以最后取最后结果的对角线元素的和。

有了上述的规则，我们仍然很难将实际问题中的导数求解出来，因此需要引入一些额外的性质来方便计算。
#### 迹的性质
* 标量的迹等于自己：$tr(x) = x$
* 转置不变：$tr(A^T)=tr(A)$ 
* 交换律：$tr(AB)=tr(BA)$，需要A,B同维度
* 加减法：$tr(A \pm B) = tr(A) \pm tr(B)$ 
* 矩阵的乘法和转置：$tr((A\odot B)^TC) = tr(A^T(B\odot C ))$，需要A,B,C同维度

#### 矩阵微分的性质
* 加减法：$d(X\pm Y) = d(X) \pm d(Y)$
* 乘法：$d(XY)=d(X)Y + Xd(Y)$
* 对元素重新排列：$d(X^\ast) = d(X)^\ast$例如转置等
* 迹：$dtr(X)=tr(dX)$
* 哈德曼积：$d(X \odot Y) = X \odot dY + dX \odot Y$
* 逐元素求导：$d\sigma(X) = \sigma^\prime(X) \odot dX$
* 逆：$dX^{-1}=-X^{-1}(dX)X^{-1}$
* 行列式：$d |X|= |X|tr(X^{-1}dX),d (ln|X|)= tr(X^{-1}dX)$

计算的原则是给标量的微分套上迹进行计算，具体计算实例可参考知乎专栏矩阵求导术[^1]

[^1]:https://zhuanlan.zhihu.com/p/24709748

