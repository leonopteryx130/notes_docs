# 关于sigmoid函数

---

### 函数本身

$$\sigma(x) = \frac{1}{1 + e^{-x}}, \quad x \in (-\infty, +\infty), \quad \sigma(x) \in (0, 1), \quad \sigma(0) = \frac{1}{2}$$

---

### 反函数

由 $$y = \dfrac{1}{1+e^{-x}}$$ 得 $$e^{-x} = \dfrac{1-y}{y}$$，取对数即得 $$x = \ln\dfrac{y}{1-y}$$。


这个函数也叫logit函数

---

### 导数

对 $$\sigma(x)$$ 直接求导：

$$\sigma'(x) = \frac{\mathrm{d}}{\mathrm{d}x}\left(\frac{1}{1 + e^{-x}}\right)$$

令 $$u = 1 + e^{-x}$$，由链式法则：

$$\sigma'(x) = -\frac{1}{(1 + e^{-x})^2} \cdot \frac{\mathrm{d}}{\mathrm{d}x}(1 + e^{-x})$$

而 $$\dfrac{\mathrm{d}}{\mathrm{d}x}(1 + e^{-x}) = -e^{-x}$$，所以：

$$\sigma'(x) = -\frac{1}{(1 + e^{-x})^2} \cdot (-e^{-x}) = \frac{e^{-x}}{(1 + e^{-x})^2}
= \frac{1}{1 + e^{-x}} \cdot \frac{e^{-x}}{1 + e^{-x}}
= \sigma(x)\bigl(1 - \sigma(x)\bigr)$$

逻辑回归中，Sigmoid 导数与交叉熵相消，$$\dfrac{\partial \ell}{\partial z} = a - y$$。$$|x|$$ 很大时导数趋近 0，易 **梯度消失**。

---

### 特点

- **S 型、单调递增**：$$x \to -\infty$$ 时趋 0，$$x \to +\infty$$ 时趋 1
- **中心对称**：$$\sigma(-x) = 1 - \sigma(x)$$
