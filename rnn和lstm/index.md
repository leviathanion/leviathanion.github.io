# RNN和LSTM(未完成)

# RNN和LSTM(待完成)
## RNN
### 问题和动机
* 自然界中很多事物都是**序列相关**的，想要理解某一时刻，必须要获取过去时刻的信息
* 过去的模型难以捕获序列信息

### idea
* 通过一个**状态变量**来存储过去的状态
* 每一时刻的状态都**由当前状态和上一时刻的状态共同决定**
* 采用**参数共享**的思想，每一时刻的参数都是相同的，简化了模型中的参数，简化了运算。
> 可以与CNN进行对比，CNN同样采用了**参数共享**(权值共享)的思想
> 
> * CNN中的**参数共享**是**空间**上的，其假设图像的底层特征与图像中的位置无关。(但是后来发现高级特征其实与位置同样有关系，需要用局部全连接层和全连接层混合来建模)
> 
> * RNN中的**参数共享**是**时间**上的，每一个时间步的参数都相同

### 模型的数学表达
$$ H_t = \sigma(W_HH_{t-1}+W{_X}X{_t}+b_h)$$
$$O_t = W_OH_t+b_q$$
这里的$\sigma$指的是非线性激活函数，在最原始的论文里用的是$Tanh$激活函数(后续改进中也有使用ReLU)，$W_H,W_X,W_O$分别是模型中的三个参数。

### 梯度消失和梯度爆炸
RNN模型中一直令人诟病的两个问题是**梯度消失和梯度爆炸**问题，这两个问题的出现严重阻碍了模型的优化。下面通过**随时间的反向传播**(**BPTT**)进行公示的推导来演示梯度消失和梯度爆炸产生的原因。
#### 符号定义
在进行推导之间，我们首先定义如下符号
|符号|解释|
|----|----|
|$K$|输入的向量维度|
|$T$|输入元素的长度|
|$H$|隐藏层单元数|
|$X= \\{ x_1,x_2,...,x_T \\} $|输入元素的按时间表示表示|
|$x_t \in \mathbb{R}^{K \times 1}$|$t$时刻RNN的输入|
|$h_t \in \mathbb{R}^{H \times 1}$|$t$时刻，隐藏层的输出|
|$o_t \in \mathbb{R}^{H \times 1}$|$t$时刻，模型的输出|
|$W_H \in \mathbb{R}^{H \times H}$|隐藏状态的权重参数|
|$W_X \in \mathbb{R}^{H \times K}$|输入的权重参数|
|$W_O \in \mathbb{R}^{K \times H}$|输出的权重参数|
|$L_t$|$t$时刻的损失函数的值|
|$L$|所有时刻损失函数的值的总和|

需要做说明的是上述符号定义并没有就具体问题进行分析，如果将其应用在不同问题上，可能有**不同的符号定义**方式，为了简单起见，本文仅以该设定为例，来讨论**梯度消失和爆炸问题**，其他问题均可采用该思想分析。

上述符号在RNN模型公式中的对应关系如下
<div>
$$\begin{cases}
h_t = tanh(W_H h_{t-1} + W_X x_{t})  \\
o_t = W_O h_t \\
L_t = f(o_t,y) \\
L = \sum\limits_{t=1}^T{L_t}
\end{cases}$$ 
<div\>

#### 反向传播
在随时间反向传播方法中，我们需要求三个偏导数，$\frac{\partial L}{\partial W_H},\frac{\partial L}{\partial W_X},\frac{\partial L}{\partial W_O}$。

##### 1.计算$\frac{\partial L_t}{\partial W_O}$
<div>
$$\begin{aligned}
\frac{\partial L_t}{\partial W_O} = 
\frac{\partial L}{\partial f(o_t,y)} 
\cdot \frac{\partial f(o_t,y)}{\partial o_t} \cdot \frac{\partial o_t}{\partial W_O}
\end{aligned}$$
<div\>

在此我们重点讨论$\frac{\partial o_t}{\partial W_O}$的计算。从上述符号表和公式我们可知$o_t$是一个$H \times 1$的列向量，$W_O$是一个$H \times K$的矩阵
##### 2.计算$\frac{\partial L_t}{\partial W_H}$
<div>
$$
\frac{\partial L_t}{\partial W_H} = \frac{\partial L}{\partial f(o_t,y)} 
\cdot \frac{\partial f(o_t,y)}{\partial o_t} \cdot \frac{\partial o_t}{\partial h_t} 
\cdot \frac{\partial h_t}{\partial W_H} 
$$<div\>
由于$h_t$是个复合函数，$h_{t-1}$同样包含$W_H$，因此根据链式求导法则和导数的乘法法则可以推出以下公式
<div>
$$
\begin{aligned}
\frac{\partial h_t}{\partial W_H} 
&= \frac{\partial h_t}{\partial W_H} 
+ \frac{\partial h_t}{\partial h_{t-1}} 
\frac{\partial h_{t-1}}{\partial W_H} \\
&= \frac{\partial h_t}{\partial W_H} 
+ \frac{\partial h_t}{\partial h_{t-1}} 
\cdot \frac{\partial h_{t-1}}{\partial W_H}
+ \frac{\partial h_t}{\partial h_{t-1}} 
\cdot \frac{\partial h_{t-1}}{\partial h_{t-2}}
\cdot \frac{\partial h_{t-2}}{\partial W_H} + ...
\end{aligned}
$$<div\>


