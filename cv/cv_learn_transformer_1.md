---
layout: page
---

- [前言](#前言)
- [注意力机制的直观认知](#注意力机制的直观认知)
  - [NLP领域](#nlp领域)
  - [CV领域](#cv领域)
- [自注意力（self-aziention）](#自注意力self-aziention)
  - [自注意力的过程和基本原理](#自注意力的过程和基本原理)
  - [矩阵表达](#矩阵表达)
  - [数学表达](#数学表达)
  - [代码分析](#代码分析)
- [多头自注意力（multi-head self-attention）](#多头自注意力multi-head-self-attention)
  - [基本原理](#基本原理)
  - [代码分析](#代码分析-1)
- [计算复杂度分析](#计算复杂度分析)
- [总结](#总结)
- [参考文献](#参考文献)

# 前言
自从Transformer[3]模型在NLP领域问世后，基于Transformer的深度学习模型性能逐渐在NLP和CV领域(Vision Transformer)取得了令人惊叹的提升。本文的主要目的是结合CV视角介绍经典Transformer模型和Vision Transformer的技术细节及基本原理，以方便读者在CV领域了解和使用Vision Transformer。由于篇幅过长，本文将分为四个部分进行介绍，包括：

（1）自注意力和多头注意力模型的原理与实现。

（2）Transformer的整体架构与实现。

（3）位置编码（positional encoding）的原理与实现。

（4）Transformer在CV领域的应用案例。

本文首先讲解第一个话题：自注意力与多头注意力模型的原理与实现。


# 注意力机制的直观认知

注意力机制最早在自然语言处理领域提出，在Transformer[3]中，**自注意力机制可以认为在学习一种关系，即一个序列中当前token与其他token之间的联系。**

## NLP领域

我们以一个自然语言处理中的序列翻译问题为例，直观地理解自注意力模型是如何发挥作用的，如下图所示。

<div class="fig figcenter fighighlight">
    <img src="{{ site.baseurl }}/asssets/learn_transformer/self_attention_visualization_0.png" width=300px>
    <img src="{{ site.baseurl }}/asssets/learn_transformer/self_attention_visualization_1.png" width=450px>
    <div class="figcaption">自注意力机制示意图[1][3]。</div>
</div>

**左侧**示意图显示了一句话中单词`it`与其他单词的联系。可以发现，通过自注意力机制，Transformer模型建立起了单词`it`和所有其他单词的联系(橙色的线条表示这种联系的权重，颜色越深表示联系约紧密)。并且，随着模型不断训练，我们发现`it`和`The animal`的联系最为密切，这正是我们想要获得的结果。通过这个例子可以看出，注意力机制有助于深度学习模型更好地理解文字序列中的每个token。

**右侧**示意图显示了一句话中任何一个token与其他所有tokens的联系，并且以连接线颜色的深浅表示这种联系的紧密程度。


## CV领域

基于上述这种自注意力机制，早在Vision Transformer之前，就有很多基于注意力机制的深度学习模型，如经典的spacial attention、channel attention、CBAM等，其核心思想是自适应地提升特征表达在空间维度或（和）通道维度对特定位置的权重，增加神经网络对特定区域的关注程度。

我们以channel attention为例，可以看到channel attention模型通过重新分配特征图通道的权重，使模型更加关注某些通道，如下图所示。

<div class="fig figcenter fighighlight">
    <img src="{{ site.baseurl }}/asssets/learn_transformer/seblock.png" width=650px>
    <div class="figcaption">Squeeze-and-Excitation block (channel attention) [5]。</div>
</div>

通过对自注意力机制在不同领域的直观感受，我们基本可以理解注意力机制到底做了些什么，并定性地感受了它为什么能够促进模型提升性能。后面，我们还将通过一些实际例子，发现自注意力机制在NLP和CV领域的一些联系，非常有意思。


# 自注意力（self-aziention）

本节，我们从自注意力的基本原理出发，首先通过一个句子翻译的例子讲解自注意力机制的计算过程及基本原理；然后，介绍前序计算过程的矩阵表达，以提升计算效率；接下来，将矩阵表达的过程整理为数学表达；最后，通过代码介绍自注意力机制的实现过程。

## 自注意力的过程和基本原理

整个自注意力的计算过程可以分为6个步骤。接下来，我们逐步介绍每个步骤的操作过程和原理。


**第一步：创建Query (q), Key (k)和Value (v)向量。** 通过下图可以看到，对于输入tokens (单词) `Thinking`和`Machines`，首先采用Embedding算法将单词编码为词向量$X_1$和$X_2$；然后，对于每个词向量$X_i$，计算该词向量对应的Query, Key和Value，分别得到$q_1$, $k_1$, $v1$和$q_2$, $k_2$, $v_2$。需要注意的是，这里的$q_i$，$k_i$和$v_i$都是学习得到的，通过对应的输入向量$X_i$乘以对应的权重矩阵$W$ (即输入词向量通过一个全连接层)。

<div class="fig figcenter ">
    <img src="{{ site.baseurl }}/asssets/learn_transformer/self_attention_step_1.png" width=500px>
    <div class="figcaption">Self-attention step 1: create Query vector, Key vector, and Value vector [1]</div>
</div>

**第二步：计算自注意力分数。** 如下图所示，计算分数时，对于任何一个词向量，我们用该向量对应的$q$与整个序列中所有的$k$进行点乘，并得到对应的分数，这里我将这个分数命名为‘*自注意力分数*’。例如，对于$X_1$，我们用其对应的$q_1$分别与该序列中的$k_1$和$k_2$相乘，则得到$q_1\cdot k_1 = 112$， $q_1 \cdot k_2 = 96$。得到的这两个分数为标量，可以用来表示当前Query对应的token与其他所有tokens之间的一个关系，且分值越大，表示关系约强烈。可以看到，$q_1$和$k_1$点乘的数值比$q_1$和$k_2$点乘的数值更大。这个结果与我们的认知是相符合的，表明在一句话中，单词`Thinking`和它自己的关系比`Thinking`和`Machines`之间的关系要更近。

<div class="fig figcenter ">
    <img src="{{ site.baseurl }}/asssets/learn_transformer/self_attention_step_2.png" width=400px>
    <div class="figcaption">Self-attention step 2：calculating a self-attention score [1]</div>
</div>


第三步和第四步的计算过程如下图所示。

**第三步：调整自注意力分数幅值。** 调整上一步自注意力分数幅值的目的是使神经网络的训练更加稳定，这在一些迭代优化算法中是经常使用的操作。在实际训练中，如果`q`和`k`计算得到的自注意力分数幅值过大，则在进行`softmax`操作时会导致梯度极小，很容易出现梯度消失现象（其原因很好理解，我们结合`softmax`函数的曲线可以看到，如果分数幅值过大，则会分布在`softmax`函数的两侧距离远点很远的位置，而这些位置的梯度极小）[3]。为了解决上述问题，一种常见的算法是第二步得到的`score`除以$1 / \sqrt{d_{k}}$，这里$d_k$表示向量$k$的维度。图中$d_k$的值为256，因此$1 / \sqrt{d_{k}}$的结果为8。

**第四步：Softmax。** 将上一步的得到的自注意力分数进行`softmax`操作，给与自注意力分数更清晰的物理意义。这样，所有分数都将被调整为正数，且所有分数之和为1。

<div class="fig figcenter ">
    <img src="{{ site.baseurl }}/asssets/learn_transformer/self_attention_step_3-4.png" width=450px>
    <div class="figcaption">Self-attention step 3-4：divide the scores and softmax. [1]</div>
</div>


第五步和第六步的计算过程如下图所示。

**第五步：将第四步计算得到经过softmax之后的分数值与对应的Value（$v$）相乘。** 这一步操作的目的是希望通过score的数值来保持我们想要关注的（相关）单词的权重，同时降低不相关单词的权重。

**第六步：将第五步得到的加权之后的Values值相加，并生成新的向量Z。** 例如，对于第一个单词`Thinking`，我们将前五步计算的最终结果$v_1$和$v_2$相加，得到$z_1$；同理，$z_2$也通过相同的方式得到单词`Machines`的输出结果。


<div class="fig figcenter ">
    <img src="{{ site.baseurl }}/asssets/learn_transformer/self_attention_step_5-6.png" width=450px>
    <div class="figcaption">Self-attention step 5-6. Step 5: multiply each value vector by the softmax score. Step 6: sum up the weighted value vectors [1]</div>
</div>

至此，我们介绍完了自注意力机制的计算过程及其每一步之后的设计动机。我们可以用一句话总结一下，self-attention的过程就是通过query和key进行相乘计算出一个关系权重，再采用这个关系权重对value进行加权求和，以提升一个序列中相关元素的权重，降低不相关元素的权重。

为提升计算效率，上述自注意力过程在实际操作中一般通过矩阵运算的方式进行并行计算。接下来，我们将介绍自注意力机制的矩阵表达。

## 矩阵表达

在矩阵表达中，我们首先将一个句子序列中所有单词的编码结果叠加为一个二维向量$X$，$X$中每一行表示一个单词。一个句子中每个单词对应的q, k, v可以直接通过一次矩阵相乘计算得到，这里用大写字母Q，K和V表示。类似地，Q, K, V的每一行代表一个单词对应的q, k, v。该过程如下图所示：

<div class="fig figcenter">
    <img src="{{ site.baseurl }}/asssets/learn_transformer/self_attention_matrix_1.png" width=280px>
    <div class="figcaption">Self-attention matrix 1 [1]</div>
</div>

接下来，将自注意力剩余的所有计算步骤全部结合在一起，如下图所示。首先，计算$Q\times K^{T}$,；然后将结果除以$\sqrt{k_{k}}$并进行softmax操作；最后，将计算结果与V相乘，直接得到最终的输出结果Z。这里Z中每一行表示一个单词对应的自注意力输出结果。

<div class="fig figcenter">
    <img src="{{ site.baseurl }}/asssets/learn_transformer/self_attention_matrix_2.png" width=450px>
    <div class="figcaption">Self-attention matrix 2 [1]</div>
</div>

至此，我们介绍完了自注意力机制的矩阵表达。

## 数学表达

根据自注意力机制[矩阵表达](#矩阵表达)的最后一个图，我们其实已经可以写出自注意力机制的数学表达了，如下：

$$
\operatorname{Attention}(Q, K, V)=\operatorname{softmax}\left(\frac{Q K^{T}}{\sqrt{d_{k}}}\right) V
$$

这里Q, K, V分表表示矩阵形式的query，key和value；$d_{k}$表示向量key的维度。


## 代码分析

```python
def attention(query, key, value, mask=None, dropout=None):
    "Compute 'Scaled Dot Product Attention'"
    d_k = query.size(-1)
    scores = torch.matmul(query, key.transpose(-2, -1)) \
             / math.sqrt(d_k)
    if mask is not None:
        scores = scores.masked_fill(mask == 0, -1e9)
    p_attn = F.softmax(scores, dim = -1)
    if dropout is not None:
        p_attn = dropout(p_attn)
    return torch.matmul(p_attn, value), p_attn
```


# 多头自注意力（multi-head self-attention）

## 基本原理


<div class="fig figcenter fighighlight">
    <img src="{{ site.baseurl }}/asssets/learn_transformer/multi_heads_1.png" width=500px>
    <div class="figcaption">Multi-Heads 1 [1]</div>
</div>

<div class="fig figcenter fighighlight">
    <img src="{{ site.baseurl }}/asssets/learn_transformer/multi_heads_2.png" width=500px>
    <div class="figcaption">Multi-Heads 2 [1]</div>
</div>

<div class="fig figcenter fighighlight">
    <img src="{{ site.baseurl }}/asssets/learn_transformer/multi_heads_3.png" width=500px>
    <div class="figcaption">Multi-Heads 3 [1]</div>
</div>

<div class="fig figcenter fighighlight">
    <img src="{{ site.baseurl }}/asssets/learn_transformer/multi_heads_4.png" width=600px>
    <div class="figcaption">Multi-Heads 4 [1]</div>
</div>


<div class="fig figcenter fighighlight">
    <img src="{{ site.baseurl }}/asssets/learn_transformer/visualization_1.png" width=350px>
    <div class="figcaption">Visualization 1 [1]</div>
</div>

<div class="fig figcenter fighighlight">
    <img src="{{ site.baseurl }}/asssets/learn_transformer/visualization_2.png" width=350px>
    <div class="figcaption">Visualization 2 [1]</div>
</div>

## 代码分析

```python
def clones(module, N):
    "Produce N identical layers."
    return nn.ModuleList([copy.deepcopy(module) for _ in range(N)])

class MultiHeadedAttention(nn.Module):
    def __init__(self, h, d_model, dropout=0.1):
        "Take in model size and number of heads."
        super(MultiHeadedAttention, self).__init__()
        assert d_model % h == 0
        # We assume d_v always equals d_k
        self.d_k = d_model // h
        self.h = h
        self.linears = clones(nn.Linear(d_model, d_model), 4)
        self.attn = None
        self.dropout = nn.Dropout(p=dropout)
        
    def forward(self, query, key, value, mask=None):
        "Implements Figure 2"
        if mask is not None:
            # Same mask applied to all h heads.
            mask = mask.unsqueeze(1)
        nbatches = query.size(0)
        
        # 1) Do all the linear projections in batch from d_model => h x d_k 
        query, key, value = \
            [l(x).view(nbatches, -1, self.h, self.d_k).transpose(1, 2)
             for l, x in zip(self.linears, (query, key, value))]
        
        # 2) Apply attention on all the projected vectors in batch. 
        x, self.attn = attention(query, key, value, mask=mask, 
                                 dropout=self.dropout)
        
        # 3) "Concat" using a view and apply a final linear. 
        x = x.transpose(1, 2).contiguous() \
             .view(nbatches, -1, self.h * self.d_k)
        return self.linears[-1](x)
```

# 计算复杂度分析

# 总结
Transformer中自注意力在大量学习一种关系，即当前token和其他tokens之间的关联性，在数学上，上述这种关系就是通过$QK^T$计算而来，二者的计算结果为一个权重系数，再将权重系数与对所有对应的$V$进行加权求和，即可得到当前token的新特征。在一个序列中，即使两个tokens之间的距离非常远，只要它们被考虑在内，就会计算它们之间的关联（有点类似协方差矩阵中随机变量内部元素之间的相关性）。这也就是文献中经常说的Transformer对长依赖(long-range dependence)处理较好。这种对全局信息的掌控能力也正是CNN缺少的，因为CNN具有局部连接特性，虽然对局部信息的处理更具优势，但在全局信息的把握上却不够充分。

当然，我们通过前文对计算复杂度分析可以看出，这种经典Transformer中的多头自注意力机制在图像上的计算复杂度为$4hwC^{2} + 2(hw)^{2}C$，这将限制Transformer在CV领域的推广，因为图像的维度要比文字序列高的多。后面，我们将介绍一种在CV中降低自注意力机制计算复杂度的策略。

最后，我们将Transformer中多头注意力模型和CV中的Attention模型进行类比，可以发现二者之间有一些关联之处。令$Q=K=src+pos$, $V=src$，其中$src$为CNN backbone提取的高维特征，$pos$为图像的位置编码，则可以认为多头注意力模型就是在channel维度对特征进行了分组（每个头为一组），并对每个组别进行spacial attention，最后再合并计算最终输出。当然，需要注意，这个过程可能不是很严谨。

# 参考文献
[1] [The Illustrated Transformer](https://jalammar.github.io/illustrated-transformer/)

[2] [The Annotated Transformer](http://nlp.seas.harvard.edu/2018/04/03/attention.html)

[3] Vaswani et al. [Attention Is All You Need](https://arxiv.org/abs/1706.03762), NIPS 2017.

[4] [DETR详解](https://zhuanlan.zhihu.com/p/386579206)

[5] Hu et al. [Squeeze-and-Excitation Networks](https://arxiv.org/abs/1709.01507), TPAMI, 2018