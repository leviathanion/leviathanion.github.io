# QRNN

# QRNN[^1](待完成)
## 问题和动机
* **标准的RNN包括门变种LSTM**等因为**无法并行计算**，因此在**长序列**的任务中性能受到了限制。
* 将**CNN**用于序列模型时
  * **并行性更好**
  * 可以**更好地扩展到长序列**
  * 但因为**最大和平均池化时假设了时间不变性**，（在一次卷积池化过程中，时间步的顺序会被忽略，移动卷积核的过程中，进行相同的池化操作，不同时间步的重要性不同同样也会被忽略）因此无法充分利用**大规模序列的顺序信息**。
* 因此作者提出了一种将**CNN和RNN混合**的模型QRNN，既能**跨时间步和小批量维度进行并行计算**，又使得**输出取决于总体顺序**。**性能更优秀且更节省时间**

## 过去的解决方法
* 将CNN应用到序列模型[^6]character-level CNN(NIPS2015)假设了时间不变性，无法利用顺序信息
![character-levelCNN模型](/QRNN/character-levelCNN.png "该模型主要通过多个卷积层和池化层堆叠，沿时间步对序列进行卷积和池化来实现序列信息的捕获")

* 音频生成领域的waveNet模型[^7]，通过五层类似于CNN的结构取得了很好效果。但这一模型在音频生成这种上一个时刻依赖很强烈的领域可能有效，但是对于NLP的其他领域并没有取得很好的效果。
    * 使用了因果卷积的假设，假设了当前时间仅仅依赖上一个时刻，但因果卷积的感受野较小，通常需要多层或者大型过滤器来增加感受野
    ![因果卷积](/QRNN/causalCNN.png "因果卷积的模型图如上所示，因果卷积的感受野较小")
    * 通过扩展卷积将感受野增大几个数量级，同时没有增加计算成本
    ![扩展卷积](/QRNN/dilatedCNN.png "扩展卷积的模型图如上所示，通过简单的方式增大了CNN的感受野")

* 将CNN与RNN结合[^8](待补充)

## 模型
QRNN模型由**两个组件**构成，类似于CNN模型中的**卷积层和池化层**。**卷积层**允许在**Minibatch和空间维度**（顺序序列）两个层面并行化训练。**池化层**允许在**Minibatch和特征维度**两个层面并行化训练，并且池化层**没有可训练的参数**。
### 卷积组件
* 为了确保对于下一个任务的预测能力，卷积操作时**不能使用未来的数据**，因此使用一种masked convolution。

> 假设输入序列为$X \in \mathbb{R}^{T \times n}$，T为时间序列的长度，n为每个输入向量$x$的维度。$Z \in \mathbb{R}^{T \times m}$为卷积操作之后隐藏层序列，其中m为每个输入对应隐藏层的维度。卷积操作如下所示
<div>
$$
\begin{aligned}
    Z &= tanh(W_z * X) 
    \\
    F &= \sigma(W_f * X) 
    \\
    O &= \sigma(W_o * X)
\end{aligned}
$$
</div>

其中$W_z,W_f,W_o \in \mathbb{R}^{k \times n \times m}$，k为卷积过滤器的长度，$ * $ 代表卷积操作，例如k取2时，对于每个元素而言，上述公式可写为

<div>
$$
\begin{aligned}
    z_t &= tanh(W^1_z x_{t-1} + W^2_z x_{t}) 
    \\
    f_t &= \sigma(W^1_f x_{t-1} + W^2_f x_{t}) 
    \\
    o_t &= \sigma(W^1_o x_{t-1} + W^2_o x_{t})
\end{aligned} 
$$
</div>

### 池化组件
* 对于池化组件而言，有多种选择，第一种选择**仅仅使用遗忘门**，第二种选择**使用遗忘门和输出门**，第三种选择**使用遗忘门，输入们和输出门**。如下所示

仅使用遗忘门
<div>
$$
h_t = f_t \odot h_{t-1} + (1 - f_t) \odot z_t 
$$
</div>

使用遗忘门和输出门
<div>
$$
\begin{aligned}
    c_t &= f_t \odot c_{t-1} + (1 - f_t) \odot z_t 
    \\
    h_t &= o_t \odot c_t 
\end{aligned}
$$
</div> 

使用遗忘门，输入门和输出门
<div>
$$
\begin{aligned}
    c_t &= f_t \odot c_{t-1} + i_t \odot z_t 
    \\
    h_t &= o_t \odot c_t 
\end{aligned}
$$
</div>   

### 变种
对于不同的任务而言可以对原始的QRNN模型进行各种改进。由于QRNN结合了CNN和RNN模型。因此**对CNN和RNN模型的改进也可以应用到QRNN上**。
#### 正则化(对于RNN的改进)
* 传统的dropout方法难以在循环连接结构上产生高效的结果
* 基于变分推理的正则化(variational inference–based dropout)[^2]，这种方式不适合于QRNN
* zoneout[^3]

#### denseNet[^4](对于CNN的改进)
* denseNet是一种在CNN模型中较为有效的连接结构,通过将层与层之间的连接二次方化，改善了梯度流和收敛性，论文的作者发现，将此连接方式用于QRNN的序列分类任务时，可以取得一定的效果提升

#### 编码解码模型
* 由于传统的编码解码器结构，只是将编码器的最后一层作为解码器的第一个池化层的输入，并没有影响到解码器的卷积层，限制了模型的表现能力。
* 作者想到将编码器的最后一层状态合并到解码器的每一个时间步的卷积操作中，可以取得较好的效果。模型被修改为以下结构,其中H为编码器的最后一层隐藏层状态
> 我个人觉得这是实验效果不错，所以写上去的理由，引入的CNN结构或者门结构都可以捕获时序信息，只要第一个被输入到卷积层或者池化层了，后面的时间步也能获取到对应的信息。这样的改进结构类似于只有编码器对后面的层使用denseNet连接，其实是改进了卷积层的CNN操作
<div>
$$
\begin{aligned}
    Z&=tanh(W_z * X + V_z * H)
    \\
    F&=\sigma(W_f * X + V_f * H)
    \\
    O&=\sigma(W_o * X + V_o * H)
\end{aligned}
$$
</div>

* 除此之外，根据论文[^5]的结果，将一个softmax卷积层放到输出前可以提升模型效果，因为不加卷积层时，仅仅是通过固定长度的向量来对所有输入进行表示。
* 通过将编码器的最后一层的每一个时间步的隐藏层状态与时间步t的门操作之前的解码器状态进行内积，然后用softmax操作，得到蕴含所有时间步的编码器隐藏状态的变量$\alpha_{at}$，然后通过此变量得到注意力总和$k_t$，最后通过门操作得到$h_t^L$，卷积操作如下所示
<div>
$$
\begin{aligned}
    \alpha_{st}&=\mathop{softmax}\limits_{all\,s}(c_t^L \cdot \tilde{h}_s^L) 
    \\
    k_t&=\sum_s\alpha_{st}\tilde{h}_s^L 
    \\
    h_t^L &= o_t \odot(W_kk_t+W_cc_t^L)
\end{aligned}
$$
</div>

## 实验
本论文在三种任务上评判了模型的性能。分别是：文档级别的情绪分类，语言模型和基于字符的神经机器翻译。





[^1]:QUASI-RECURRENT NEURAL NETWORKS
[^2]:Yarin Gal and Zoubin Ghahramani. A theoretically grounded application of dropout in recurrent neural networks. In NIPS, 2016.
[^3]:David Krueger, Tegan Maharaj, János Kramár, Mohammad Pezeshki, Nicolas Ballas, Nan Rosemary Ke, Anirudh Goyal, Yoshua Bengio, Hugo Larochelle, Aaron Courville, et al. Zoneout: Regularizing RNNs by Randomly Preserving Hidden Activations. 
[^4]:Densely connected convolution networks.(CVPR2017)
[^5]:Neural machine translation by jointly learning to align and translate(ICLR 2015)
[^6]:Character-level Convolutional Networks for Text Classification(NIPS2015)
[^7]:WaveNet: A Generative Model for Raw Audio
[^8]:Fully Character-Level Neural Machine Translation without Explicit Segmentation

