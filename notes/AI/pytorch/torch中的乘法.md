# torch中的乘法

PyTorch 里「乘法」有两类：**矩阵乘**（线性代数意义）和**按元素乘**（Hadamard 积）。运算符上也有对应：`@` / `torch.matmul` 走矩阵乘，`*` / `torch.mul` 走按元素乘。

### 矩阵乘法

#### 1. `torch.mm` — 专用于二维矩阵

- 输入必须都是 **2D**：`(n, m)` × `(m, p)` → `(n, p)`
- 不支持广播；需要批量时用 `bmm` 或 `matmul`

```python
import torch

A = torch.randn(2, 3)
B = torch.randn(3, 4)
C = torch.mm(A, B)   # (2, 4)
# 等价：A @ B
```

#### 2. `torch.bmm` — 批量矩阵乘法（三维）

- 输入必须都是 **3D**：`(b, n, m)` × `(b, m, p)` → `(b, n, p)`
- `b` 为 batch 维，两张量的 batch 大小必须相同，**无广播**

```python
A = torch.randn(10, 2, 3)   # 10 个 2×3 矩阵
B = torch.randn(10, 3, 4)   # 10 个 3×4 矩阵
C = torch.bmm(A, B)         # (10, 2, 4)
```

#### 3. `torch.matmul` — 通用矩阵乘（推荐日常使用）

按输入维度自动分派，并支持广播：

| 输入维度 | 行为 | 输出 |
| --- | --- | --- |
| 1D × 1D | 点积 | 标量 |
| 1D × 2D | 向量 × 矩阵 | 1D |
| 2D × 1D | 矩阵 × 向量 | 1D |
| 2D × 2D | 同 `mm` | 2D |
| ≥3D | 对最后两维做矩阵乘，前面维广播 | 同广播规则 |

```python
# 二维
torch.matmul(torch.randn(2, 3), torch.randn(3, 4))  # (2, 4)

# 批量 + 广播：左侧无 batch，右侧有
A = torch.randn(2, 3)
B = torch.randn(10, 3, 4)
C = torch.matmul(A, B)   # (10, 2, 4)

# 运算符 @ 等价于 matmul
C2 = A @ B
```

Attention / 线性层里常见写法：

```python
# Q: (batch, seq, d_k), K: (batch, seq, d_k)
# scores: (batch, seq, seq)
scores = torch.matmul(Q, K.transpose(-2, -1))
```

#### 4. `torch.mv` — 矩阵 × 向量

- 输入：矩阵 `(n, m)`、向量 `(m,)` → 向量 `(n,)`
- 等价于 `torch.matmul(mat, vec)`，但不接受更高维

```python
M = torch.randn(3, 4)
v = torch.randn(4)
y = torch.mv(M, v)   # (3,)
```

### 向量乘法

#### 1. `torch.dot` — 内积（点积）

- 两个 **一维** 同长度向量 → **标量**
- $$\mathbf{a}\cdot\mathbf{b}=\sum_i a_i b_i$$
- 更高维请用 `torch.matmul` 或 `torch.tensordot`

```python
a = torch.tensor([1., 2., 3.])
b = torch.tensor([4., 5., 6.])
torch.dot(a, b)   # tensor(32.)  = 1*4+2*5+3*6
```

#### 2. `torch.outer` — 外积（得到矩阵）

- 两个一维向量 $$\mathbf{a}\in\mathbb{R}^n$$、$$\mathbf{b}\in\mathbb{R}^m$$ → 矩阵 $$(n, m)$$
- 即 $$\mathbf{a}\mathbf{b}^{\mathsf{T}}$$，$$C_{ij}=a_i b_j$$
- **不是**三维叉积；叉积用 `torch.cross`（见[向量的内积和外积](../../数学/理论数学/向量的内积和外积.md)）

```python
a = torch.tensor([1., 2., 3.])
b = torch.tensor([4., 5.])
torch.outer(a, b)
# tensor([[ 4.,  5.],
#         [ 8., 10.],
#         [12., 15.]])
```

#### 3. `torch.mul` / `*` — 按元素相乘

- 对应位置相乘，支持广播，结果形状由广播规则决定
- 与矩阵乘完全不同：`A * B` ≠ `A @ B`

```python
A = torch.tensor([[1., 2.], [3., 4.]])
B = torch.tensor([[10., 20.], [30., 40.]])
torch.mul(A, B)   # [[10, 40], [90, 160]]
A * B             # 同上
```