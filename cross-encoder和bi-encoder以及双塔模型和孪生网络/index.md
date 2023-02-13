# Cross-Encoder和Bi-Encoder以及双塔模型和孪生网络

# cross-encoder
* 将query与document交叉输入到encoder中，得到一个得分
* 使用该得分作为相似度

# bi-encoder
* 将query与document分别输入到两个encoder中，得到两个得分
* 使用两个得分进行相似度计算

![Bi_vs_Cross-Encoder](/encoder/Bi_vs_Cross-Encoder.png "Bi_vs_Cross-Encoder")

# 双塔模型(伪孪生网络)
* 来自于搜广领域的名词
* 将query和document分别输入到两个**独立**的encoder中，得到两个得分
* 使用两个得分进行相似度计算

![双塔模型](/encoder/双塔模型.jpeg "双塔模型")

# 孪生网络(siamese)
* 将query和document分别输入到两个**不独立，共享权重，相同**的encoder中，得到两个得分
* 使用两个得分进行相似度计算

![孪生网络](/encoder/孪生网络.jpg "孪生网络")



