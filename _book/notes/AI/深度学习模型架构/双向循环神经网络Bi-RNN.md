# 双向循环神经网络Bi-RNN

### 模型图

<div align="center">
    <img src=./双向循环神经网络Bi-RNN.png width=80% />
</div>

### 核心思想

在普通 RNN 的基础上再**复制一份**，得到两个方向相反的网络：

- **正向 RNN**：从左到右读，依次处理词 $$x_1, x_2, \ldots, x_T$$
- **反向 RNN**：从右到左读，依次处理词 $$x_T, x_{T-1}, \ldots, x_1$$

前向传播时，两个 RNN 是**完全独立**的网络，互不干扰。下面公式里的下标 $$t$$ 一律表示**词的位置**（第 $$t$$ 个词），不是各自内部的时间步。

- $$h_t^{\rightarrow}$$：正向网络在**位置 $$t$$** 的隐藏状态（已看过 $$x_1,\ldots,x_t$$）
- $$h_t^{\leftarrow}$$：反向网络在**位置 $$t$$** 的隐藏状态（已看过 $$x_t,\ldots,x_T$$）

### 输出如何拼接

- **整句表示**：拼的是两个方向各自「读完整句」后的状态。在**词位置**编号下，正向读完落在位置 $$T$$，反向读完落在位置 $$1$$，所以是 $$[h_T^{\rightarrow}; h_1^{\leftarrow}]$$。
- **每个词的表示**：按**同一个词位置**拼 $$[h_t^{\rightarrow}; h_t^{\leftarrow}]$$。


### 应用到 Encoder-Decoder 中

Seq2Seq 里用 Bi-RNN 做 Encoder 时，正向与反向隐藏状态会拼接，语义向量维度变为原来的 **2H**。和 Decoder 对接时通常有三种做法：

1. **线性投影**：用 $$W \in \mathbb{R}^{H \times 2H}$$ 把 $$[h_T^{\rightarrow}; h_1^{\leftarrow}]$$ 压回维度 $$H$$，再作为 Decoder 的初始状态。
2. **对齐维度**：直接把 Decoder 隐藏层设为 $$2H$$，与 Encoder 输出对齐，不再压缩。
3. **Attention**：不再只用单个固定向量 C；Decoder 每步对编码器各位置的 $$[h_t^{\rightarrow}; h_t^{\leftarrow}]$$（维度 $$2H$$）做加权求和，再决定当前输出。
