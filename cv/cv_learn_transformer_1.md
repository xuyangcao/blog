---
layout: page
---

- [前言](#前言)
- [注意力机制](#注意力机制)
- [自注意力（self-attention）](#自注意力self-attention)
- [多头自注意力（multi-head self-attention）](#多头自注意力multi-head-self-attention)
- [总结](#总结)
- [参考文献](#参考文献)

# 前言
自从Transformer[3]模型在NLP领域问世后，基于Transformer的深度学习模型性能逐渐在NLP和CV领域取得了令人惊叹的提升。本文的主要目的是结合CV视角来介绍Transformer模型的技术细节及基本原理，以方便读者在CV领域了解和使用Transformer。由于篇幅过长，本文将分为四个部分进行介绍，包括：
（1）自注意力和多头注意力模型的原理与实现。
（2）Transformer的整体架构与实现。
（3）位置编码（positional encoding）的原理与实现。
（4）Transformer在CV领域的应用案例。

本文首先讲解第一个话题：自注意力与多头注意力模型的原理与实现。

# 注意力机制
在CV领域，注意力机制并不陌生，如经典的spacial attention、channel attention、SE Block、CBAM等，其核心思想是自适应地提升特征表达在空间维度或（和）通道维度对特定位置的权重，增加神经网络对特定区域的关注程度。

在Transformer中，自注意力机制可以直观地理解为在学习一种关系，即一句话中当前字符与其他字符之间联系的紧密程度。

自注意力机制可以认为是Transformer的核心技术，掌握了自注意力和多头注意力模块，Transformer模型就不难理解了。

# 自注意力（self-attention）


# 多头自注意力（multi-head self-attention）
# 总结
我们发现，Transformer中的自注意力在大量的学习一种关系，即当前token和其他tokens之间的关联性，即使两个tokens之间的距离非常远，只要他们被考虑在内，就会计算它们之间的关联。（有点类似协方差矩阵中随机变量内部元素之间的相关性）

上述这种关系就是通过$QK^T$计算而来，二者的计算结果为一个权重系数，再将权重系数与对所有对应的$V$进行加权求和，即可得到当前token的新特征。

最后，我们将Transformer中多头注意力模型和CV中的Attention模型进行类比，可以发现二者之间有一些关联之处。令$Q=K=src+pos$, $V=src$，其中$src$为CNN backbone提取的高维特征，$pos$为图像的位置编码，则可以认为多头注意力模型就是在channel维度对特征进行了分组（每个头为一组），并对每个组别进行spacial attention，最后再合并计算最终输出。当然，需要注意，这个过程可能不是很严谨。


# 参考文献
[1] [The Illustrated Transformer](https://jalammar.github.io/illustrated-transformer/)

[2] [The Annotated Transformer](http://nlp.seas.harvard.edu/2018/04/03/attention.html)

[3] Vaswani et al. [Attention Is All You Need](https://arxiv.org/abs/1706.03762), NIPS 2017.

[4] [DETR详解](https://zhuanlan.zhihu.com/p/386579206)