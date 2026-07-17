# Intersection over Union 交并比

IoU（Intersection over Union，交并比）衡量两个区域（常为边界框）的重叠程度，定义为交集面积与并集面积之比：

$$
\mathrm{IoU}(A,B)=\frac{|A\cap B|}{|A\cup B|}=\frac{|A\cap B|}{|A|+|B|-|A\cap B|}
$$

取值范围为 $$[0,1]$$：完全重合为 1，无重叠为 0。

<div align="center">
    <img src=./IoU交并比1.jpg width=60% />
</div>

在目标检测中，常令 $$A$$ 为预测框、$$B$$ 为 ground-truth。IoU 既用来衡量定位质量，也常用阈值判定检测是否命中（例如 $$\mathrm{IoU}\ge 0.5$$ 视为正确检测）。

<div align="center">
    <img src=./IoU交并比2.jpg width=60% />
</div>

