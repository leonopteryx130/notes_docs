# n-gram

### 基本思想

该模型基于这样一种假设，第 $$N$$ 个词的出现只与前面 $$N-1$$ 个词相关，而与其它任何词都不相关，整句的概率就是各个词出现概率的乘积。这些概率可以通过直接从语料中统计 $$N$$ 个词同时出现的次数得到。常用的是二元的 Bi-Gram 和三元的 Tri-Gram。

设句子 $$W = w_1 w_2 \cdots w_m$$，由链式法则有：

$$
P(W) = P(w_1) \, P(w_2 \mid w_1) \, P(w_3 \mid w_1, w_2) \cdots P(w_m \mid w_1, \ldots, w_{m-1})
$$

n-gram 用马尔可夫假设近似上式：只保留最近的 $$n-1$$ 个词作为条件。

### 模型定义

n=1，一元模型（unigram model）

表达式：

$$
P(W) = \prod_{i=1}^{m} P(w_i), \qquad
P(w_i) = \frac{C(w_i)}{\sum_{w} C(w)}
$$

说明：假设各词相互独立，不考虑上下文。$$C(w_i)$$ 为词 $$w_i$$ 在语料中出现的次数；$$\sum_{w} C(w)$$ 对词表中所有词 $$w$$ 的出现次数求和，等于语料的总词数（tokens 数）。因此 $$P(w_i)$$ 就是该词的相对频率。最粗糙，但实现简单，常作 baseline。

---

n=2，二元模型（bigram model）

表达式：

$$
P(W) = P(w_1) \prod_{i=2}^{m} P(w_i \mid w_{i-1}), \qquad
P(w_i \mid w_{i-1}) = \frac{C(w_{i-1}, w_i)}{C(w_{i-1})}
$$

说明：当前词只依赖前一个词。$$C(w_{i-1}, w_i)$$ 为二元组 $$(w_{i-1}, w_i)$$ 共现次数；$$C(w_{i-1})$$ 为前一个词 $$w_{i-1}$$ 在语料中出现的次数，作归一化分母（即「以 $$w_{i-1}$$ 开头的二元组」总数）。比一元模型能捕捉局部词序，是最常用的 n-gram。

---

n=3，三元模型（trigram model）

表达式：

$$
P(W) = P(w_1) \, P(w_2 \mid w_1) \prod_{i=3}^{m} P(w_i \mid w_{i-2}, w_{i-1})
$$

$$
P(w_i \mid w_{i-2}, w_{i-1}) = \frac{C(w_{i-2}, w_{i-1}, w_i)}{C(w_{i-2}, w_{i-1})}
$$

说明：当前词依赖前两个词。$$C(w_{i-2}, w_{i-1}, w_i)$$ 为三元组共现次数；$$C(w_{i-2}, w_{i-1})$$ 为前两个词构成的二元组 $$(w_{i-2}, w_{i-1})$$ 出现次数，作归一化分母。上下文更丰富，但稀疏性更严重——许多三元组在语料中从未出现，条件概率为 0，通常需要平滑（如 add-k、Katz backoff、Kneser-Ney）。

### 一般形式

$$
P(w_i \mid w_{i-n+1}, \ldots, w_{i-1}) = \frac{C(w_{i-n+1}, \ldots, w_i)}{C(w_{i-n+1}, \ldots, w_{i-1})}
$$

$$n$$ 越大，对长距离依赖建模越强，但数据稀疏与参数量增长也越快；实践中 $$n=2$$ 或 $$3$$ 最常见。

### 举个例子

使用二元模型。以下 mini-corpus 来自 Jurafsky & Martin《Speech and Language Processing》第 3 章（句首加 `<s>`，句尾加 `</s>`）：

```
<s> I am Sam </s>
<s> Sam I am </s>
<s> I do not like green eggs and ham </s>
```

书中给出的部分 bigram 概率：

$$
\begin{aligned}
&P(\text{I} \mid \langle s \rangle) = \tfrac{2}{3},\quad
P(\text{Sam} \mid \langle s \rangle) = \tfrac{1}{3},\quad
P(\text{am} \mid \text{I}) = \tfrac{2}{3}, \\
&P(\langle /s \rangle \mid \text{Sam}) = \tfrac{1}{2},\quad
P(\text{Sam} \mid \text{am}) = \tfrac{1}{2},\quad
P(\text{do} \mid \text{I}) = \tfrac{1}{3}
\end{aligned}
$$

完整统计如下（只列出计算时会用到的项）：

| 一元 $$C(w)$$ | 次数 | 二元 $$C(w_{i-1}, w_i)$$ | 次数 |
| --- | --- | --- | --- |
| `<s>` | 3 | `<s>, I` | 2 |
| I | 3 | `<s>, Sam` | 1 |
| am | 2 | `I, am` | 2 |
| Sam | 2 | `I, do` | 1 |
| do | 1 | `am, Sam` | 1 |
| not | 1 | `am, </s>` | 1 |
| like | 1 | `Sam, I` | 1 |
| green | 1 | `Sam, </s>` | 1 |
| eggs | 1 | `do, not` | 1 |
| and | 1 | `not, like` | 1 |
| ham | 1 | `like, green` | 1 |
| `</s>` | 3 | `green, eggs` | 1 |
| | | `eggs, and` | 1 |
| | | `and, ham` | 1 |
| | | `ham, </s>` | 1 |

求较长句 *I do not like green eggs and ham* 的概率：

$$
\begin{aligned}
&P(\text{I do not like green eggs and ham}) \\
&= P(\text{I} \mid \langle s \rangle)
  \, P(\text{do} \mid \text{I})
  \, P(\text{not} \mid \text{do})
  \, P(\text{like} \mid \text{not})
  \, P(\text{green} \mid \text{like})
  \, P(\text{eggs} \mid \text{green})
  \, P(\text{and} \mid \text{eggs})
  \, P(\text{ham} \mid \text{and})
  \, P(\langle /s \rangle \mid \text{ham}) \\
&= \frac{2}{3} \cdot \frac{1}{3} \cdot \frac{1}{1} \cdot \frac{1}{1} \cdot \frac{1}{1} \cdot \frac{1}{1} \cdot \frac{1}{1} \cdot \frac{1}{1} \cdot \frac{1}{1}
= \frac{2}{9}
\end{aligned}
$$

作为对照，短句 *I am Sam*：

$$
P(\text{I am Sam})
= \frac{2}{3} \cdot \frac{2}{3} \cdot \frac{1}{2} \cdot \frac{1}{2}
= \frac{1}{9}
$$

注意：语料极小，许多二元组只出现 1 次，条件概率多为 $$1$$；真实语料上稀疏性会严重得多，需要平滑。


### 应用场景

- **语言模型 / 输入法**：估计下一词概率，做联想输入、自动补全。
- **语音识别 / OCR**：用语言模型对候选转写打分，纠正声学或视觉识别错误。
- **拼写纠错**：比较编辑候选在上下文中的 n-gram 概率，选更通顺的词。
- **机器翻译（统计时代）**：对目标语句打分，偏好更「像人话」的译文。
- **中文分词等序列切分**：用字/词 n-gram 评估切分方案的优劣。
- **文本特征**：把 unigram / bigram 当作特征，用于分类、检索、查重等。

深度学习语言模型已基本取代 n-gram 做端到端任务，但 n-gram 仍常作 baseline，并在资源受限或需要可解释统计时使用。

