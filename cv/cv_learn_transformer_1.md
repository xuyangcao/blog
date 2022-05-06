---
layout: page
---

- [前言](#前言)
- [注意力机制的直观认知](#注意力机制的直观认知)
  - [NLP领域](#nlp领域)
  - [CV领域](#cv领域)
- [自注意力（self-attention）](#自注意力self-attention)
  - [基本原理](#基本原理)
  - [矩阵表达](#矩阵表达)
  - [计算复杂度分析](#计算复杂度分析)
  - [代码分析](#代码分析)
- [多头自注意力（multi-head self-attention）](#多头自注意力multi-head-self-attention)
  - [基本原理](#基本原理-1)
  - [计算复杂度分析](#计算复杂度分析-1)
  - [代码分析](#代码分析-1)
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


# 自注意力（self-attention）

## 基本原理

<div class="fig figcenter fighighlight">
    <img src="{{ site.baseurl }}/asssets/learn_transformer/self_attention_step_1.png" width=500px>
    <div class="figcaption">Self-attention step 1 [1]</div>
</div>

<div class="fig figcenter fighighlight">
    <img src="{{ site.baseurl }}/asssets/learn_transformer/self_attention_step_2.png" width=400px>
    <div class="figcaption">Self-attention step 2 [1]</div>
</div>

<div class="fig figcenter fighighlight">
    <img src="{{ site.baseurl }}/asssets/learn_transformer/self_attention_step_3-4.png" width=450px>
    <div class="figcaption">Self-attention step 3-4 [1]</div>
</div>

<div class="fig figcenter fighighlight">
    <img src="{{ site.baseurl }}/asssets/learn_transformer/self_attention_step_5-6.png" width=450px>
    <div class="figcaption">Self-attention step 5-6 [1]</div>
</div>

## 矩阵表达

<div class="fig figcenter fighighlight">
    <img src="{{ site.baseurl }}/asssets/learn_transformer/self_attention_matrix_1.png" width=280px>
    <div class="figcaption">Self-attention matrix 1 [1]</div>
</div>

<div class="fig figcenter fighighlight">
    <img src="{{ site.baseurl }}/asssets/learn_transformer/self_attention_matrix_2.png" width=450px>
    <div class="figcaption">Self-attention matrix 2 [1]</div>
</div>


## 计算复杂度分析
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

## 计算复杂度分析

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

# 总结
Transformer中的自注意力在大量的学习一种关系，即当前token和其他tokens之间的关联性，在数学上，上述这种关系就是通过\\(QK^T\\)计算而来，二者的计算结果为一个权重系数，再将权重系数与对所有对应的\\(V\\)进行加权求和，即可得到当前token的新特征。在一个序列中，即使两个tokens之间的距离非常远，只要它们被考虑在内，就会计算它们之间的关联（有点类似协方差矩阵中随机变量内部元素之间的相关性）。这也就是文献中经常说的Transformer对long-range dependence处理较好，也正是CNN缺少的部分，因为CNN具有局部连接的特性，对局部信息的处理更具优势。

当然，我们通过前文对计算复杂度分析可以看出，这种经典Transformer中的多头自注意力机制在图像上的计算复杂度为\\(4hwC^{2} + 2(hw)^{2}C\\)，这将限制Transformer在CV领域的推广，因为图像的维度要比文字序列高的多。后面，我们将介绍一种在CV中降低自注意力机制计算复杂度的策略。

最后，我们将Transformer中多头注意力模型和CV中的Attention模型进行类比，可以发现二者之间有一些关联之处。令\\(Q=K=src+pos\\), \\(V=src\\)，其中\\(src\\)为CNN backbone提取的高维特征，\\(pos\\)为图像的位置编码，则可以认为多头注意力模型就是在channel维度对特征进行了分组（每个头为一组），并对每个组别进行spacial attention，最后再合并计算最终输出。当然，需要注意，这个过程可能不是很严谨。

# 参考文献
[1] [The Illustrated Transformer](https://jalammar.github.io/illustrated-transformer/)

[2] [The Annotated Transformer](http://nlp.seas.harvard.edu/2018/04/03/attention.html)

[3] Vaswani et al. [Attention Is All You Need](https://arxiv.org/abs/1706.03762), NIPS 2017.

[4] [DETR详解](https://zhuanlan.zhihu.com/p/386579206)

[5] Hu et al. [Squeeze-and-Excitation Networks](https://arxiv.org/abs/1709.01507), TPAMI, 2018