# SCRN

# SCRN模型[^1]
## 问题和动机
* 前馈神经网络中的**时滞神经网络**[^2]，通过一个**固定长度的最近历史窗口**的结构，达到了能够建模**序列数据**的目的。但也有以下缺点
  * 固定长度的窗口难以学习到**长距离依赖**
  * 只能接受**线性增长**的参数代价
* 简单的递归神经网络(SRN)由于**梯度消失**问题同样难以学习到**长距离依赖**
  * 部分非线性激活函数，例如**sigmoid**使得任何地方的梯度都接近零。(可以通过使用**ReLu**等激活函数来部分解决这个问题)
  * BPTT算法出现**矩阵连乘现象**，如果矩阵**特征值很小(小于1)，梯度会迅速收敛到0。**
* 过去的带有**语境特征**(**contextual feature**)的SRU使用NLP的**预训练技术**将其引入，并**没有**将其作为循环网络的一部分来**训练**
* 本文提出一种较为**简单，将语境特征作为模型的一部分来训练**的模型

## 过去的解决方法
* 用**Hessian-Free**来代替**SGD**
* **LSTM**模型
* 用**语境特征**(**contextual feature**)进行**预训练**来引入长距离的上下文信息

## Idea
* (trick)使用**分层softmax函数**来代替原本的softmax函数，因为**计算softmax函数的normalization项通常是性能瓶颈**(**会损失表现性能**)
* 使用**梯度重归一来避免梯度爆炸**(相当于**梯度裁剪**)
* **非线性激活会导致梯度消失**，完全连接的隐藏层在每个时间步都会完全改变状态(**参数矩阵完全改变**)，因此增加一个参数矩阵为**单位矩阵**，且**没有非线性激活**的隐藏层。如下式所示
$$s_t = s_{t-1}+Bx_t$$
* 最后的模型如下所示

<div>
$$
\begin{aligned}
s_t &= (1-\alpha)Bx_t + \alpha s_{t-1}
\\
h_t &= \sigma(Ps_t+Ax_t+Rh_{t-1}) 
\\
y_t &= f(Uh_t+Vs_t) 
\end{aligned}
$$</div>

* 上述公式固定了$\alpha$，因此只能学习固定的时间尺度。如果将其设为可训练的话，那么就能从不同的时间延迟上学习。如下所示

<div>
$$
\begin{aligned}
s_t &= (I-Q)Bx_t + Q s_{t-1} 
\\
h_t &= \sigma(Ps_t+Ax_t+Rh_{t-1}) 
\\
y_t &= f(Uh_t+Vs_t) 
\\
diag(Q) &= \sigma(\beta)
\end{aligned}
$$
</div>

> 论文作者提到，只要还使用标准隐藏层，固定$\alpha$与将其作为可训练的参数区别不大。


## 实验
实现中将$\alpha$固定为0.95，BPTT的步数为50，普通SRN步数为10。每向前进行5步就进行一次梯度下降，batchsize设为32，初始学习率为0.05，当验证误差不再减少时，每次训练完成后，将学习率除以1.5。
### Penn Treebank Corpus数据集
#### 固定$\alpha$参数
**在参数较少的小数据集上，SCRN和LSTM具有相当的性能**，当LSTM具有更多的模型参数，大约是4倍。与"leaky neurons"相比，SCRN也有较大改善。结果如下图所示

![Penn Treebank Corpus数据集](/SCRN/PennTreebankCorpus固定参数.png "固定参数")

#### 学习自适应权重
当隐藏层的**参数大小很小**时，**学习自适应权重是有效的**，但当**参数规模逐渐变大**的时候，并**没有带来显著的改善**。结果如下图所示

![Penn Treebank Corpus数据集](/SCRN/PennTreebankCorpus训练参数.png "学习自适应权重")


### Text8数据集
SCRN随着隐藏层和context参数的增加，性能会逐步提升。如下图所示

![Text8数据集](/SCRN/Text8自我比较.png)


**当参数规模较小时，SCRN优于LSTM，但参数规模变大之后还是LSTM的性能更好**，如下图所示

![Text8数据集](/SCRN/Text8与LSTM对比.png "SRN，LSTM与SCRN的对比")

## 结论
**当参数规模受到限制时，SCRN会大大优于LSTM**。但当参数规模变大时，有相似的性能。但所有那些模型都不能真正学习到长时记忆。本文还在github上开源了**代码**[^3]

## 思考中的问题
- [ ] 单位矩阵在模型中起到的作用(推导反向传播)



[^1]:Learning lo
nger memory in recurrent neural networks.
[^2]:Rumelhart, David E, Hinton, Geoffrey E, and Williams, Ronald J. Learning internal representations
by error propagation. Technical report, DTIC Document, 1985.
[^3]: http://github.com/facebook/SCRNNs

