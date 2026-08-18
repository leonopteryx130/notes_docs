# Multi-Head Attention

### 基本思想

单头缩放点积注意力只有一组 $$W_Q, W_K, W_V$$，所有位置之间的关系被压进**同一个**子空间，学到的往往是一种「平均」的看法。

多头的做法很直接：**把同一套缩放点积注意力复制 $$h$$ 份**，每头用自己的 $$W_Q^{(i)}, W_K^{(i)}, W_V^{(i)}$$ 把 $$Q, K, V$$ 投到一个更低维的空间，各自独立算一遍 Attention，得到 $$h$$ 个输出，再**拼接**起来，最后乘一个 $$W_O$$ 投回原来的维度。

直觉：同一句话可以同时问好几件不同的事——谁修饰谁、代词指谁、远距离依赖——每头负责一种「看」法，拼回去才是完整表示。

> 单头计算见 [Attention](./Attention.md)、[self-Attention](./self-Attention.md)。多头只是把那一套机制平行跑 $$h$$ 次。


### 模型图

<div align="center">
    <img src=./Multi-head.png width=70% />
</div>

自下而上对应四步：

1. **Linear（每头一组）**：$$Q, K, V$$ 各乘自己的投影矩阵。图里叠在一起的盒子就是 $$h$$ 个头。
2. **Scaled Dot-Product Attention**：每头内部就是普通的缩放点积注意力，彼此独立。
3. **Concat**：把 $$h$$ 个头的输出在特征维上拼成一条更长的向量。
4. **Linear（$$W_O$$）**：把拼接结果投回模型维度，得到最终输出。


### 公式

单头仍是：

$$\mathrm{Attention}(Q, K, V) = \mathrm{softmax}\!\left(\frac{Q K^\top}{\sqrt{d_k}}\right) V$$

多头是在这套计算外面再套一层：**一份原始 QKV，配 $$h$$ 组投影矩阵，得到 $$h$$ 个 Attention 输出。**

#### 1. 一组原始 QKV

输入经过嵌入（以及位置编码）后，得到基础的查询、键、值矩阵，形状都是 $$\mathrm{seq\_len} \times d_{\mathrm{model}}$$：

$$Q,\; K,\; V \in \mathbb{R}^{\mathrm{seq\_len} \times d_{\mathrm{model}}}$$

后面每一头都是拿**同一份** $$Q, K, V$$ 去投影，不会再生成第二份「原始」QKV。

#### 2. 有多组 $$W^Q, W^K, W^V$$

假设有 $$h$$ 个头，就有 $$h$$ 组**独立**的投影矩阵：

| 头 | 投影矩阵 |
|----|----------|
| 第 1 组 | $$W_1^Q,\ W_1^K,\ W_1^V$$ |
| 第 2 组 | $$W_2^Q,\ W_2^K,\ W_2^V$$ |
| $$\vdots$$ | $$\vdots$$ |
| 第 $$h$$ 组 | $$W_h^Q,\ W_h^K,\ W_h^V$$ |

每组形状：$$W_i^Q, W_i^K \in \mathbb{R}^{d_{\mathrm{model}} \times d_k}$$，$$W_i^V \in \mathbb{R}^{d_{\mathrm{model}} \times d_v}$$。参数互不共享，所以每头学到的「怎么投影、怎么看」可以不一样。

#### 3. 得到多个 Attention 输出

用第 $$i$$ 组矩阵去投影原始 QKV，再各自做缩放点积注意力：

$$\mathrm{head}_1 = \mathrm{Attention}(Q W_1^Q,\ K W_1^K,\ V W_1^V)$$

$$\mathrm{head}_2 = \mathrm{Attention}(Q W_2^Q,\ K W_2^K,\ V W_2^V)$$

$$\vdots$$

$$\mathrm{head}_h = \mathrm{Attention}(Q W_h^Q,\ K W_h^K,\ V W_h^V)$$

得到 $$h$$ 个独立输出。严格说每个 $$\mathrm{head}_i$$ 是矩阵，形状 $$\mathrm{seq\_len} \times d_v$$：序列里每个位置都会输出一个向量。

最后把 $$h$$ 个头在特征维上拼起来，再乘 $$W_O$$ 投回模型维度：

$$\mathrm{MultiHead}(Q, K, V) = \mathrm{Concat}(\mathrm{head}_1, \ldots, \mathrm{head}_h)\, W_O$$

$$W_O \in \mathbb{R}^{h d_v \times d_{\mathrm{model}}}$$

原论文里通常取 $$d_k = d_v = d_{\mathrm{model}} / h$$。例如 $$d_{\mathrm{model}} = 512$$、$$h = 8$$ 时，每头只有 64 维。参数总量和单头用满 512 维差不多，但得到了 8 组不同的注意力分布。

> Self-Attention 里这份原始 $$Q, K, V$$ 都从同一条序列来；Encoder-Decoder 交叉注意力里，$$Q$$ 来自解码器、$$K, V$$ 来自编码器。多头公式对两种情况都一样，只是原始 QKV 的来源不同。
