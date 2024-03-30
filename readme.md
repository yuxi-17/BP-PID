# BP-PID算法的Python实现

一个简单的BP-PID算法的python实现，BP-PID算法相当于两层全连接神经网络生成PID的三个参数。

## BP神经网络的输入

通常来说，输入为4个值，包括：目标值、当前的输出、error、1（作为偏置）。

## 激活函数

第一层为tanh，第二层为sigmoid（因为要保证输出的PID参数大于0），但是sigmoid范围在0-1之间，范围较窄，所以有些论文还另外给每一个参数都乘上一个系数，这里我只给kp*10，其余保持不变（不同场景应该不一样，需要根据具体情况调整）。

## PID

采用增量式PID算法，输出需要限幅。

## 反向传播

传递函数输出对输入的梯度使用下面的公式模拟：
$$
\frac{\partial y(t)}{\partial e(t)}=\frac{y(t)-y(t-1)}{e(t)-e(t-1)}
$$
由于其数值大小对梯度的影响不大，可以通过调整学习率弥补这一偏差。因此只需要了解其方向即可，所以使用sign函数。

对于矩阵相乘y = x*W，如果需要求x的梯度，将y的梯度乘上W的转置即可；求W的梯度，只需要将x的转置乘y的梯度即可。

学习率lr和一阶动量alfa需要根据具体情况调整。

## 效果

BP-PID相比于PID来说，超调量有一定的改善，但是其稳定时间相对较长。

此外，从参数整定上来看，PID需要Kp、Ki、Kd三个参数，但是BP-PID也需要根据情况调整学习率、一阶动量等。