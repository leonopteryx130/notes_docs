# Encoder-Decoder架构

### 架构核心思想

**Encoder-Decoder** 是经典的 **Seq2Seq** 框架，常用于机器翻译等任务：

1. **Encoder**：读入整个输入序列，压缩为固定长度的**语义编码 C**（上下文向量）。
2. **Decoder**：以 C 为条件，从 `<Start>` 起**自回归**地逐步生成输出，每步把上一步的输出作为下一步输入。

适用于输入与输出**长度不一致**的序列任务：翻译、摘要、对话生成等。

### 架构图

<div align="center">
    <img src=./Encoder-Decoder架构.jpg width=90% />
</div>

以法语 `Elle aime la Chine .` → 英语 `She loves China .` 为例：左侧编码，中间为语义编码 C，右侧解码生成。

### 模型详解

**Encoder**（图中绿色节点）：词经 Embedding 层转为向量，再依次过 RNN，隐藏状态从左向右传递。读完整句后，**最后时刻的隐藏状态**即为语义编码 $$C = h_T^{enc}$$。

**语义编码 C**（紫色虚线框）：连接编解码的桥梁，用单向量概括整句语义；解码首步时作为 $$h_{t-1}^{dec}$$（即初始隐藏状态 $$h_0^{dec} = C$$）传入解码器。

**Decoder**（图中蓝色节点）：首步接收 C 与 `<Start>`，之后每步以上一步输出为输入，依次生成 `She` → `loves` → `China` → `.`，直到 `<EOS>` 或达到最大长度。

$$h_t^{dec} = f(W_{dec} y_{t-1} + U_{dec} h_{t-1}^{dec}) \qquad P(y_t) = \text{softmax}(V h_t^{dec})$$

- **训练**：解码器用真实目标词作下一步输入（Teacher Forcing），计算交叉熵损失。
- **推理**：每步取概率最大的词作为输出，再喂回解码器。
