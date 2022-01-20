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
