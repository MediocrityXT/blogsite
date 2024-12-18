---
title: 交叉熵=KL散度=最大似然估计
date: '2021-07-02 10:09:03'
math: true
excerpt: 损失函数中常用到的KL散度、交叉熵，与最大似然估计，本质上是一样的啊！就是希望我预测出来的与我的标注的概率分布越接近越好。
tags:
- math
- deeplearning
---

损失函数中常用到的KL散度、交叉熵，与最大似然估计，本质上是一样的啊！
就是希望我预测出来的与我的标注的概率分布越接近越好。


比较x变量在p概率分布下的熵、KL散度和交叉熵的数学定义
$$
H(X)=-\sum{p(x)\log p(x)}\\\\
H(p,q)=-\sum{p(x)\log q(x)}\\\\
D_{KL}(p||q)=-\sum{p(x)\log \frac{p(x)}{q(x)}}
$$
会发现
$$
D_{KL}(p||q)=H(p,q)-H(p)
$$
即`KL散度 = 交叉熵 - 熵`

# 从编码的角度理解交叉熵

**熵**的意义是：对任意一个随机变量A编码所需要的**最小字节数**，也就是使用哈夫曼编码根据A的概率分布对A进行编码所需要的字节数；

**KL散度**的意义是：使用随机变量B的最优编码方式对随机变量A编码所需要的**额外字节数**，具体来说就是使用哈夫曼编码却根据B的概率分布对A进行编码，所需要的编码数比A编码所需要的最小字节数多的数量；

**交叉熵**的意义是：使用随机变量B的最优编码方式对随机变量A编码所需要的**字节数**，具体来说就是使用哈夫曼编码却根据B的概率分布对A进行编码，所需要的编码数。

在损失函数中，ground-truth当做随机变量A，这个熵是常数。

最大似然估计的公式与这种情况下的交叉熵相同。

> 坏了，我现在自己看不懂了。。。2022/9/12。
>
> 不过使用交叉熵作为loss的原因有一条“当使用Softmax函数作为输出节点的激活函数的时候，一般使用交叉熵作为损失函数。由于Softmax函数的数值计算过程中，很容易因为输出节点的输出值比较大而发生数值溢出的现象，在计算交叉熵的时候也可能会出现数值溢出的问题。为了数值计算的稳定性，TensorFlow提供了一个统一的接口，将Softmax与交叉熵损失函数同时实现，同时也处理了数值不稳定的异常，使用TensorFlow深度学习框架的时候，一般推荐使用这个统一的接口，避免分开使用Softmax函数与交叉熵损失函数。”

# Pytorch loss fucntion

2023/7/12 面试时考到了用torch手写交叉熵loss和huber loss这两个类

这里可以用`jupytext --to markdown RewriteLossFunctions.ipynb `将jupyter notebook的调试代码转成md形式放上来，当然，运行结果是无法保存的了。

## CrossEntropyLoss

（公式扒自pytorch文档）

假设输入$x$是具有[B, C, N]三个个维度，B是minibatch的样本数目，C表示token的class数目，N表示单个样本内token数。weights是C维度的，对于特定的类乘以特定的权重。ignore_index表示特定的类被忽略，不为交叉熵贡献，但是实际上这些类仍然参与了softmax运算。

> 样本是1D的句子，所以样本是1D的句子，所以除了B，C多一个N维度。
>
> 对于高维输入如2D图片的每个像素做CELoss，输入x是[B,C,d1,d2]。

y有两种可选的输入：

1. y是[N]维度的class indices，每个元素作为类标签都在[0,C)范围内。此时交叉熵计算方法如下
   $$
   \ell(x, y) = L = \{l_1,\dots,l_N\}^\top, \\
   l_n = - w_{y_n} \log \frac{\exp(x_{n,y_n})}{\sum_{c=1}^C \exp(x_{n,c})}
   \cdot \mathbb{1}\{y_n \not= \text{ignore\_index}\}
   $$

   这个公式可以用nn.LogSoftmax和nn.NLLLoss简单实现。

   

2. 可能由于进行了label smoothing，标签y是[N,C]维度的class probabilities，每个元素作为该token属于某类的概率，C维度元素之和都为1。此时交叉熵计算方法如下。这个情况不考虑ignore_index。【torch 1.7.1版本还未支持这种输入。torch版本和CUDAToolkit版本强相关所以不能随便升级，所以这里不考虑这种标签输入】
   $$
   \ell(x, y) = L = \{l_1,\dots,l_N\}^\top,\\
   l_n = - \sum_{c=1}^C w_c \log \frac{\exp(x_{n,c})}{\sum_{i=1}^C \exp(x_{n,i})} y_{n,c}
   $$

> 注：L可以用mean()或者sum()的方法进行reduction。这里默认是mean()，即在batch维度所有样本取平均。【对于高维输入，同样应该是所有元素取平均】

## 代码

### 输入初始化

```python
import torch
import torch.nn as nn

# batch size B, sequence length N, class nums C
B = 2
N = 2
C = 4
logits = torch.randn(B,C,N)
labels= torch.randint(0,C,(B,N))
weights = torch.randn(C)
ignore_index = torch.randint(0,C,()).item()

# 注释这下面两行可以获得随机weights和ignore_index
weights = None
ignore_index = -100
print(weights)
print(ignore_index)
print(logits)
print(labels)
```

### 简单实现

通过`nn.LogSoftmax`, `nn.NLLLoss`两行代码实现交叉熵Loss。

```python
class CrossEntropyLoss_Simple(nn.Module):

    def __init__(self, weight = None,ignore_index= -100):
        super().__init__()
        self.weight = weight
        self.ignore_index = ignore_index
        # LogSoftmax实现有特殊处理，避免了上下溢出
            # log_softmax与softmax的区别在哪里？ - 秋乏术的回答 - 知乎
            # https://www.zhihu.com/question/358069078/answer/2845782335
        self.log_softmax = nn.LogSoftmax(dim=1)
        # 注意kwargs必须写key，省略key传进去不起作用
        self.nll_loss = nn.NLLLoss(weight = weight,ignore_index = ignore_index)
        
    def forward(self,logits,labels):
        # logits [B,C,N], labels[B,N]
        log_prob = self.log_softmax(logits)
        loss = self.nll_loss(log_prob, labels)
        return loss
```

### 底层实现

使用基本pytorch操作实现交叉熵。但没有设计防止softmax的上下溢出。

```python
class CrossEntropyLoss(nn.Module):
    def __init__(self,weight = None,ignore_index= -100):
        super().__init__()
        self.weight = weight
        self.ignore_index = ignore_index
    
    def forward(self,logits,labels):
        C = logits.shape[1]
        # logits [B,C,N], labels[B,N]
        s = logits.exp().sum(dim=1,keepdim=True).repeat(1,C,1)
        # log_result [B,C,N]
        log_prob = (logits.exp()/s).log()
        if self.weight != None:
            weight = self.weight.view(1,C,1).repeat(logits.shape[0],1,logits.shape[2])
            log_prob *= weight

            if self.ignore_index in range(0,C):
            # 后面算mean的时候weight要忽略一个值
                weight[:,self.ignore_index] = 0
            
            # 用标签gather weight w_yn
            w_y = torch.gather(weight, dim = 1, index = labels.unsqueeze(1))
            weightSum = w_y.view(-1).sum()

        # labels [B,N] 直接作为index进行gather
            # 或者labels也可以转成one hot向量作为mask进行masked_select（或者点乘），转成one-hot的方法：
            # nn.functional.one_hot() / scatter_ / torch.where(index == target, ones, zeros)

        if self.ignore_index in range(0,C):
            # 直接在log_prob该列全部置0
            log_prob[:,self.ignore_index] = 0
            

        l = - torch.gather(log_prob, dim=1, index=labels.unsqueeze(1)).squeeze()
        # reduction默认为在B维度上平均
        if self.weight == None:
            loss = l.view(-1).mean()
        else:
            loss = l.view(-1).sum()/weightSum
        return loss
```

### 实验

随机logits, labels, weights和ignore_index情况下，两种实现和`nn.CrossEntropyLoss`返回值都完全相同。

```python
loss1 = nn.CrossEntropyLoss(weight=weights,ignore_index=ignore_index)
print(loss1(logits,labels))
loss2 = CrossEntropyLoss_Simple(weight=weights,ignore_index=ignore_index)
print(loss2(logits,labels))
loss3 = CrossEntropyLoss(weight=weights,ignore_index=ignore_index)
print(loss3(logits,labels))
# tensor(1.1413)
# tensor(1.1413)
# tensor(1.1413)
```

## NLLLoss

输入x是所有标签上的log probabilities。负对数概率Loss是最大似然估计的思想。
$$
\ell(x, y) = L = \{l_1,\dots,l_N\}^\top, \\
l_n = - w_{y_n} x_{n,y_n}, \quad
w_{c} = \text{weight}[c] \cdot \mathbb{1}\{c \not= \text{ignore\_index}\}
$$