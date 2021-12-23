# IRNN

# IRNN模型[^1]
## 问题和动机
* **梯度消失**和**梯度爆炸**的问题导致RNN模型难以学习到**远距离依赖**
* 过去的解决方法依赖于复杂的**优化技术**和**网络架构**
* 提出一种较为**简单**的方式进行优化

## 过去的解决方法
* 用**Hessian-Free**来代替**SGD**[^2] [^3]（Hessian-Free可以关注到曲率）
>虽然有次改进，但并不常用，原因如下
>  * 比SGD基础上的方法**更难实现**
>  * 拟牛顿法需要**更复杂的计算和内存开销**
>  * 目标函数可能**非凸**
>  * 求得了更高精度的解可能**不利于泛化**
>  * 二阶法求得更高精度的解，但也**需要更高精度的数据**，深度学习数据本身存在很多**随机误差**，可能导致优化不稳定甚至失败
> * 当使用了**梯度裁剪**[^5]并关注了**参数初始化**[^4]时，**带动量的SGD**与**HF方法**可以相当
* 最成功的改进**LSTM**

  虽然能解决长距离依赖，但本文作者认为此结构并不是最优的结构

## Idea
* 使用**ReLU**(Rectified Linear Units)激活函数
* (trick)通过**单位矩阵**或者**它的缩放版本**来初始化RNN中的权重矩阵

>作者认为以下工作与本文工作类似
>* 本文作者认为其参数初始化方法与文献[^6]中的方法相同，主要区别在于本文仅仅将单位矩阵用在初始化上，而且使用了ReLU激活函数。
>
>* scaled identity initialization同样在文献[^7]中被提出，但没使用ReLU激活函数。
> 
>* 本文工作与文献[^8]研究**正交矩阵初始化**同样类似。

## 实验
>实验中发现了下述炼丹技巧
> * 为LSTM设置更**高的forget gate bias**能更好的解决长距离依赖关系
> 
> * 参数应用高斯随机初始化时使用**文献[^9]的值**时效果更好。

实验中IRNN中非递归权重使用均值为0，标准差为0.001的高斯分布来初始化。分别展示了IRNN，标准LSTM，使用tanh激活的RNN和使用ReLU激活的RNN在4个实验上的结果。

### The Adding Problem实验
![Adding Problem实验](/IRNN/addingProblem.png "输入由两部分组成，第一部分是0-1范围内均匀分布的值，第二部分是掩码，最后的结果是经掩码后的两值得和")
通过在固定序列中随机抽取两个值，将其值求和作为输出，对此问题进行回归。使用MSE来评价效果，当一直预测值为1时，MSE为0.1767。实验结果如下所示
![Adding Problem实验结果](/IRNN/addingProblemResult.png "可以发现IRNN呈现出与LSTM相当的效果，但带有ReLU激活的RNN效果为何一直这么差?")

### MNIST Classification from a Sequence of Pixels实验
实验通过将图片的784个像素顺序输入网络中，在完全学习完784个像素后，再进行图片的分类。一次每个网络的循环步长为784。除了对原始图片进行预测的实验之外，还对图片的像素进行固定的随机重新排列后进行了第二次实验。
![MNIST实验结果](/IRNN/MNISTResult.png "可以发现，IRNN性能最好，超过LSTM，但带有ReLU激活的RNN效果为何一直这么差?")

### Language Modeling实验
![LanguageModel实验结果](/IRNN/LanguageModel.png "4层IRNN与LSTM表现相当(同时LSTM的参数是单层IRNN的四倍)")

### Speech Recognition实验
在该实验中作者发现使用单位矩阵初始化效果较差，本文猜测的原因如下
* 正常IRNN很难忘记过去的信息
* 难以专注当前的输入
> 我觉得等于没说，根据模型和结果猜原因

因此本文提出了补救办法，并认为其可以作为**不需要长距离依赖时**模型的补救办法
* 用一个**小标量和单位矩阵的乘积来初始化**(本文中使用的是0.01I)

![Speech Recognition实验结果](/IRNN/SpeechRecognitionResult.png "iRNN模型与LSTM相当，比标准RNN效果好")


## 论文Insight
* 使用**单位矩阵初始化循环参数**(可能缓解梯度爆炸的问题)
* 使用**ReLU激活**(可以缓解梯度消失的问题)
* 二者**共同作用**可使得RNN模型性能得到改善
* 在**不需要长期依赖**时，可以使用**小标量与I的乘积**来进行初始化，类似于LSTM中的**遗忘门**机制

## 思考中的问题
- [ ] 超参初始化问题，xariv，He，和本文中[^9]提到的初始化的关系和效果
- [ ] 单位矩阵在模型中起到的作用(推导反向传播)
- [ ] 使用ReLU激活的RNN效果比tanh还差的原因

[^1]:https://arxiv.org/abs/1504.00941 (A Simple Way to Initialize Recurrent Networks of Rectified Linear Units)

[^2]:Learning recurrent neural networks with Hessian-Free optimization. In ICML, 2011.

[^3]:Deep learning via Hessian-free optimization. In Proceedings of the 27th International Conference on Machine Learning, 2010.


[^4]:On the importance of initialization and momentum in deep learning. In Proceedings of the 30th International Conference on Machine
Learning, 2013.

[^5]:On the difficulty of training recurrent neural networks.

[^6]:Learning longer memory in recurrent neural networks

[^7]:Parsing with compositional vector grammars

[^8]:Exact solutions to the nonlinear dynamics of learning in deep linear neural networks

[^9]:Random Walk Initialization for Training Very Deep Feedforward Networks
