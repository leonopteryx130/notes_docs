# Attention

### 基本思想

注意力的本质：**用 Query 去查一组 (Key, Value)，按相似度加权取出 Value**。

| 角色 | 含义 | 直觉 |
|------|------|------|
| **Query (Q)** | 当前想「问」什么 | 此刻的问题 / 解码器当前状态 |
| **Key (K)** | 用来被比对的「索引」 | 每条候选信息的标签 |
| **Value (V)** | 真正要取出的内容 | 与 Key 对应的信息本体 |

计算流程统一为三步：

1. **打分**：对每个 Key，算它与 Query 的相似度 $$s_i = F(Q, K_i)$$
2. **归一化**：把分数变成权重（通常 Softmax），$$\alpha_i = \mathrm{softmax}(s)_i$$，且 $$\sum_i \alpha_i = 1$$
3. **加权求和**：用权重聚合 Value，得到注意力输出

$$\mathrm{Attention}(Q, K, V) = \sum_i \alpha_i\, V_i$$

常见的相似度函数 $$F$$：点积 $$Q^\top K$$、缩放点积 $$Q^\top K / \sqrt{d}$$、余弦相似度，以及 Bahdanau 的加性注意力。

> Key 与 Value 可以相同（很多序列模型里都是），也可以不同；一般有多少个 Key，就有多少个对应的 Value。


### 从 Bahdanau Attention 中抽离

<div align="center">
    <img src=./Attention.jpg width=80% />
</div>

Bahdanau Attention 是注意力在 Seq2Seq 上的一个具体实例。把它的角色剥出来，正好对应上图的 Q、K、V 与三个阶段：

| 抽象概念 | 在 Bahdanau 中对应 |
|----------|-------------------|
| **Query** | 解码器上一时刻状态 $$s_{t-1}$$（「我现在要生成第 $$t$$ 个词，关心什么」） |
| **Key** | 编码器各位置注解 $$h_i$$（用来比对「哪里相关」） |
| **Value** | 同样是 $$h_i$$（Bahdanau 里 Key = Value） |
| **Attention Value** | 上下文向量 $$c_t$$ |

对应图中三个阶段：

#### 阶段 1：相似度计算

用 $$F(Q, K_i)$$ 得到未归一化分数 $$s_1, s_2, \ldots$$。在 Bahdanau 中即加性注意力：

$$s_i = e_{t,i} = v^\top \tanh(W_s s_{t-1} + W_h h_i)$$

#### 阶段 2：类 Softmax 归一化

将 $$s_i$$ 转为注意力权重 $$a_1, a_2, \ldots$$（即 $$\alpha_{t,i}$$）：

$$a_i = \alpha_{t,i} = \frac{\exp(s_i)}{\sum_j \exp(s_j)}$$

#### 阶段 3：加权求和

用权重对 Value 加权求和，得到最终的 Attention Value（即 $$c_t$$）：

$$\mathrm{Attention\ Value} = c_t = \sum_i a_i\, V_i = \sum_i \alpha_{t,i}\, h_i$$

因此：**Bahdanau 不是另一套机制，而是「Query = 解码器状态、Key/Value = 编码器状态、$$F$$ = 加性打分」的特例**；上图则是可复用到 Transformer 等模型的通用骨架。
