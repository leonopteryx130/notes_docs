# pytorch里的tensor

PyTorch 里的 **Tensor**，本质上和 NumPy 的 `ndarray` 非常像——都是一个多维数组，用来存储数值、做矩阵运算。但它最核心、最不一样的地方在于 **自动求导（Autograd）** 和 **GPU 加速**。

### 和 NumPy 的共同点

- 都支持多维数组、切片、广播、矩阵乘法等操作
- API 风格相近，甚至可以用 `.numpy()` / `torch.from_numpy()` 互相转换（注意共享内存）

```python
import torch
import numpy as np

# 从 Python 列表创建
t = torch.tensor([[1., 2.], [3., 4.]])
print(t.shape)   # torch.Size([2, 2])
print(t.dtype)   # torch.float32

# 和 NumPy 互转
arr = np.array([[1, 2], [3, 4]], dtype=np.float32)
t2 = torch.from_numpy(arr)   # 共享底层内存，改一个另一个也会变
t3 = torch.tensor(arr)       # 拷贝一份，互不影响
```

### 核心灵魂：自动求导（Autograd）

这是 PyTorch 作为深度学习框架的根本。普通的数组只是存储数据，而 PyTorch 的 Tensor 会**记住自己是怎么来的**。

- 设置 `requires_grad=True` 时，Tensor 会记录所有操作（加减乘除等），形成**计算图**
- 调用 `.backward()` 后，梯度会自动累积到 `.grad` 属性里
- 你不需要手动推导偏导数，框架替你做了

**对比**：NumPy 完全不具备这个能力。

#### Demo：一元函数求导

设 $$y = x^2$$，当 $$x = 3$$ 时，$$\frac{dy}{dx} = 2x = 6$$。

```python
import torch

x = torch.tensor(3.0, requires_grad=True)
y = x ** 2

y.backward()          # 从 y 反向传播，计算 dy/dx
print(x.grad)         # tensor(6.)  ✓
```

#### Demo：链式法则（多层运算）

设 $$z = (2x + 1)^2$$，$$x = 2$$ 时，$$\frac{dz}{dx} = 2(2x+1) \cdot 2 = 20$$。

```python
x = torch.tensor(2.0, requires_grad=True)

# 每一步运算都会被记录
a = 2 * x + 1
z = a ** 2

z.backward()
print(x.grad)         # tensor(20.)
```

#### Demo：向量输入与 `backward` 的 `gradient` 参数

标量 loss 可以直接 `.backward()`；若输出是向量，需要传入一个与输出同形状的权重向量（通常全 1），表示对每个输出分量求导的系数。

```python
x = torch.tensor([1.0, 2.0, 3.0], requires_grad=True)
y = x ** 2            # tensor([1., 4., 9.])

# y 不是标量，需要传入 gradient
y.backward(torch.ones_like(y))
print(x.grad)         # tensor([2., 4., 6.])  即 dy_i/dx_i = 2*x_i
```

#### 训练时的典型用法

```python
import torch
import torch.nn as nn

# 简单线性模型 y = wx + b
x = torch.tensor([[1.0], [2.0], [3.0]])
y_true = torch.tensor([[2.0], [4.0], [6.0]])

w = torch.tensor([[1.0]], requires_grad=True)
b = torch.tensor([[0.0]], requires_grad=True)

y_pred = x @ w + b
loss = ((y_pred - y_true) ** 2).mean()

loss.backward()       # 自动算出 d(loss)/d(w) 和 d(loss)/d(b)
print(w.grad, b.grad)
```

```

### 硬件加速：无缝在 CPU 和 GPU 间切换

PyTorch Tensor 的数据可以和 **GPU 显存**直接绑定。

- 调用 `.to('cuda')` 或 `.cuda()`，矩阵运算切换到显卡上并行计算，速度可提升几十到上百倍
- 切换设备后，**调用代码完全不用改**（加法还是加法，乘法还是乘法）

**对比**：NumPy 只能跑在 CPU 上，无法直接利用 GPU。

#### 注意

- CPU Tensor 和 GPU Tensor 不能直接运算，会报错，需要先 `.to()` 到同一设备
- `.cuda()` 等价于 `.to('cuda')`；多卡时可用 `.to('cuda:1')` 指定显卡
- 从 GPU 取回 CPU：`t.cpu()` 或 `t.to('cpu')`；转 NumPy：`t.cpu().numpy()`（GPU 上的 Tensor 不能直接 `.numpy()`）

### 小结

| | NumPy `ndarray` | PyTorch `Tensor` |
|---|---|---|
| 多维数组运算 | ✓ | ✓ |
| 自动求导 | ✗ | ✓（`requires_grad` + `backward`） |
| GPU 加速 | ✗ | ✓（`.to(device)`） |
