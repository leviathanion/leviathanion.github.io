# KGT5

# KGT5模型[^1]
## Novelty
* 使用encoder-decoder架构来进行KGE
* 将KGC任务转换了seq2seq任务
* 使用三元组评分和自回归相交互的方式
* 模型大小减少了98%，推理时间加快
* 不需要进行超参数调整

## 问题和动机
* 面对大图，KGE应该满足以下特性
    * 可扩展性：模型大小与推理时间和实体数量无关
    * 高质量：达到良好的性能
    * 通用性：适用于多种任务KGC和QA等
    * 简单性：模型应该为单一模块
* 各种模型的四个特性如下图，无法满足所有
    * 传统的KGE模型为每个实体创建一个嵌入，规模可能大
    * KG-BERT使用了Cross-Encoder，费时间
![四个特性](/KGT5/四个特性.png "不同模型的四个特性")
* 将链路预测作为一个seq2seq问题，使用Transformer进行预训练，QA任务对该模型进行微调
    * 可扩展性：组合实体表示，进行自回归(不是对所有实体打分)
    * 高质量：在两个任务上达到了最优
    * 通用性：同一模型可以在多个数据集上用于KGC和KGQA
    * 简单性：一个模型
![KGT5整体](/KGT5/KGT5整体.png "KGT5整体")


## 相关工作

## idear

## 模型

## 实验

[^1]:Sequence-to-Sequence Knowledge Graph Completion and Question

