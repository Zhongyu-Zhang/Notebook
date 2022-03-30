# 训练深度学习模型时，LOSS出现NAN

首先需要使用debug工具判断问题出现的原因，主要判断以下几点：

- 部分loss计算出现NAN还是模型的输出出现NAN。

- 训练刚开始便出现NAN还是训练了100个epoch之后出现NAN。

## 原因一：loss function定义出错

主要体现在计算loss时部分出现NAN，或者模型的输出出现了NAN

**解决方案**：分母、被开方数、log底数加一个极小数`1e-8`。

**注**：需特别注意，被开方数如果取值过小，超出精度，会导致梯度无法反传，因此需要加一个极小数。

## 原因二：batch size和学习率设置的过大

多体现在训练了几十个epoch之后loss出现NAN。

**解决方案**：调小batch size和学习率。

## 原因三：模型反传时，出现梯度爆炸现象

主要出现在训练循环神经网络问题上。

**解决方案**：按照如下代码设计梯度截断。

`loss.backward()`
`nn.utils.clip_grad_norm_(model.parameters(), max_norm=5, norm_type=2)`
`optimizer.step()`

