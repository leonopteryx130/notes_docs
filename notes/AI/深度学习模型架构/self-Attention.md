# self-Attention

### 传统 Attention

Q、K、V 来自**不同来源**。典型场景是 Encoder-Decoder：解码器当前状态当 Query，去查编码器各位置（Key / Value）。

直觉：我现在要说下一句话（Q），回头翻源句（K、V），按相关程度把有用信息抽出来。

公式（缩放点积）：

$$\mathrm{Attention}(Q, K, V) = \mathrm{softmax}\!\left(\frac{Q K^\top}{\sqrt{d}}\right) V$$

其中 $$Q$$ 与 $$K, V$$ 不必同源。Bahdanau 则是同一套三步（打分 → Softmax → 加权求和），只是打分用加性网络，且 $$K = V =$$ 编码器隐状态。

> 详见 [Attention](./Attention.md)、[Bahdanau Attention](./Bahdanau_Attention.md)。


### Self-Attention

把 Q、K、V **全部改成从同一条序列 $$X$$ 里来**。设 $$X \in \mathbb{R}^{T \times d}$$（$$T$$ 个位置，每个 $$d$$ 维）。最简写法直接令 $$Q = K = V = X$$（无投影）：

$$\mathrm{SelfAttn}(X) = \mathrm{softmax}(X X^\top)\, X$$

$$X X^\top$$ 是 $$T \times T$$ 的相似度矩阵（每个位置对所有位置打分），**对每一行** Softmax 后加权求和，输出仍是 $$T \times d$$：每个位置得到一个「看过全句之后」的新向量。

> 这是**无投影**的直觉写法，对应下面图 1–2 的手算。真正的 Transformer Self-Attention 还会：
> 1. 用三个可学习矩阵投影：$$Q = X W_Q,\ K = X W_K,\ V = X W_V$$，同源不等于三个矩阵一模一样；
> 2. 点积除以 $$\sqrt{d_k}$$（$$d_k$$ 是投影后的维度），避免维度一高 Softmax 过尖。
>
> 完整式：$$\mathrm{softmax}\!\left(\dfrac{(X W_Q)(X W_K)^\top}{\sqrt{d_k}}\right)(X W_V)$$。无投影版是它在 $$W_Q = W_K = W_V = I$$、且不缩放时的特例。

#### 核心思想

让**每个位置都当一次 Query**，去看整句所有位置（含自己）。不再是「一个外部问句去查一段文本」，而是**序列内部互相看**：任意两词之间一步可达，不依赖 RNN 逐步传递。

### 公式拆解

以「早上好」三个字为例，每个字已经是一个 5 维向量。先只算「早」这一行，对应无投影公式 $$\mathrm{softmax}(X X^\top)\, X$$ 的第一行。

#### 1. 打分与归一化

<div align="center">
    <img src=./self-Attention1.png width=90% />
</div>

「早」当 Query，三个字都当 Key，做点积：

| | 早 `[1, 2, 1, 2, 1]` | 上 `[1, 1, 3, 2, 1]` | 好 `[3, 1, 2, 1, 1]` |
|---|---|---|---|
| **早** `[1, 2, 1, 2, 1]` | $$1+4+1+4+1=11$$ | $$1+2+3+4+1=11$$ | $$3+2+2+2+1=10$$ |

得到「早」这一行的原始分数 $$[11,\ 11,\ 10]$$。再归一化成和为 1 的权重 $$[0.4,\ 0.4,\ 0.2]$$。

> 图中为方便演示做了取整。真实 Softmax$$(11, 11, 10) \approx (0.42,\ 0.42,\ 0.16)$$，趋势一样：更像「早 / 上」，不太像「好」。

含义：处理「早」时，大约 40% 看自己、40% 看「上」、20% 看「好」。

矩阵视角：$$X X^\top$$ 一次算出所有位置两两的点积（这里是 $$3 \times 3$$）；图里只画出了第一行。对每一行单独 Softmax，保证**每个 Query 自己的权重和为 1**。

#### 2. 用权重聚合 Value

<div align="center">
    <img src=./self-Attention2.png width=90% />
</div>

无投影时 Value 就是原来的三个向量。用刚得到的权重做加权求和，得到「早」的新表示：

$$0.4 \times [1, 2, 1, 2, 1] + 0.4 \times [1, 1, 3, 2, 1] + 0.2 \times [3, 1, 2, 1, 1] = [1.4,\ 1.4,\ 2,\ 1.8,\ 1]$$

这不再是「早」的原始向量，而是按相关程度混进了「上」和「好」的信息。

对「上」「好」各当一次 Query，重复同样两步，得到另外两行。三行拼起来就是 $$\mathrm{SelfAttn}(X)$$，形状仍是 $$3 \times 5$$。

#### 3. 放到真实句子里看

<div align="center">
    <img src=./self-Attention3.png width=90% />
</div>

左列某个词当 Query（图中高亮的是 `cross_`），连到右列各词；色条深浅就是注意力权重。句子 *The animal didn't cross the street because it was too tired.* 里，一个词可以一步直接看向句中任意远处，不必像 RNN 那样一格一格传。

右侧多列颜色对应 **多头注意力**（Multi-Head）：同一套机制平行算好几遍，每头用自己的 $$W_Q, W_K, W_V$$，学不同的「看」法，最后把各头输出拼回去。

### 详解 X 的线性变换

<div align="center">
    <img src=./self-Attention4.png width=90% />
</div>

图 1–2 直接用 $$X$$ 当 Q、K、V，三种角色完全相同。Transformer 里会先把 $$X$$ 乘上三个**不同的**可学习矩阵，让「用来提问」「用来被比对」「真正取出来的内容」分开学：

$$Q = X W_Q,\quad K = X W_K,\quad V = X W_V$$

图中：$$X$$ 是 $$2 \times 4$$（2 个位置，每个 4 维），三个 $$W$$ 都是 $$4 \times 3$$，于是 $$Q, K, V$$ 都是 $$2 \times 3$$。一般地：

| 矩阵 | 形状 | 含义 |
|------|------|------|
| $$X$$ | $$T \times d$$ | 输入（位置数 × 模型维） |
| $$W_Q, W_K, W_V$$ | $$d \times d_k$$ | 三个独立的投影，参数可学习 |
| $$Q, K, V$$ | $$T \times d_k$$ | 每个位置变成 $$d_k$$ 维的三种角色 |

为什么要投影：

1. **三种角色解耦**：同一个词当 Query 和当 Value 时，需要的信息往往不同（「我在找什么」vs「我能提供什么」）。
2. **降维 / 多头**：$$d_k$$ 可以小于 $$d$$。多头时通常把 $$d$$ 拆成 $$h$$ 份，每头用自己的一组 $$W$$，再把各头输出拼回 $$d$$ 维。
3. **可学习**：$$W$$ 随训练更新，模型自己决定怎样把 $$X$$ 变成更好比对、更好聚合的空间。

投影之后，计算与图 1–2 **完全一样**，只是把 $$X$$ 换成 $$Q, K, V$$，并补上缩放：

$$\mathrm{Attention}(Q, K, V) = \mathrm{softmax}\!\left(\frac{Q K^\top}{\sqrt{d_k}}\right) V$$

$$Q K^\top$$ 仍是 $$T \times T$$：第 $$i$$ 行就是位置 $$i$$ 对全句的打分，Softmax 后乘 $$V$$，得到每个位置的新向量。

> 除以 $$\sqrt{d_k}$$ 的原因：点积的方差大约随维度线性变大，维数一高分数会被拉得很极端，Softmax 几乎变成 one-hot，梯度也没了。除掉之后训练才稳。图 1 维数小、数字也小，手算时可以先不除。
>
> 解码器里的 Self-Attention 还会加 **因果掩码**（位置 $$i$$ 不能看后面的词），否则生成时会偷看未来。编码器没有这个限制。
