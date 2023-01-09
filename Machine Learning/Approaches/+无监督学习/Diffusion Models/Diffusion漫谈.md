## [Diffusion漫谈](https://kexue.fm/archives/9164)

可以看这一整个系列，介绍了很多diffusion的概念

### [DDPM = 贝叶斯 + 去噪](https://kexue.fm/archives/9164)

DDPM的流程：
$$\boldsymbol{x} = \boldsymbol{x}_0 \rightleftharpoons \boldsymbol{x}_1 \rightleftharpoons \boldsymbol{x}_2 \rightleftharpoons \cdots \rightleftharpoons \boldsymbol{x}_{T-1} \rightleftharpoons \boldsymbol{x}_T = \boldsymbol{z}$$
求解目标：
$$p(\boldsymbol{x}_{t-1}|\boldsymbol{x}_t) = \frac{p(\boldsymbol{x}_t|\boldsymbol{x}_{t-1})p(\boldsymbol{x}_{t-1})}{p(\boldsymbol{x}_t)}$$
但是其中只有$p(\boldsymbol{x}_t|\boldsymbol{x}_{t-1})$是已知的，因此DDPM退而求其次，在给定$\boldsymbol{x}_0$的条件下使用贝叶斯定理：
$$
\begin{aligned}
p(\boldsymbol{x}_{t-1}|\boldsymbol{x}_t, \boldsymbol{x}_0)
&= \frac{p(\boldsymbol{x}_t|\boldsymbol{x}_{t-1}, \boldsymbol{x}_0)p(\boldsymbol{x}_{t-1}|\boldsymbol{x}_0)}{p(\boldsymbol{x}_t|\boldsymbol{x}_0)} \\
&\propto \exp \Big(-\frac{1}{2} \big(\frac{(\mathbf{x}_t - \sqrt{\alpha_t} \mathbf{x}_{t-1})^2}{\beta_t} + \frac{(\mathbf{x}_{t-1} - \sqrt{\bar{\alpha}_{t-1}} \mathbf{x}_0)^2}{1-\bar{\alpha}_{t-1}} - \frac{(\mathbf{x}_t - \sqrt{\bar{\alpha}_t} \mathbf{x}_0)^2}{1-\bar{\alpha}_t} \big) \Big) \\
&= \exp \Big(-\frac{1}{2} \big(\frac{\mathbf{x}_t^2 - 2\sqrt{\alpha_t} \mathbf{x}_t \color{blue}{\mathbf{x}_{t-1}} \color{black}{+ \alpha_t} \color{red}{\mathbf{x}_{t-1}^2} }{\beta_t} + \frac{ \color{red}{\mathbf{x}_{t-1}^2} \color{black}{- 2 \sqrt{\bar{\alpha}_{t-1}} \mathbf{x}_0} \color{blue}{\mathbf{x}_{t-1}} \color{black}{+ \bar{\alpha}_{t-1} \mathbf{x}_0^2}  }{1-\bar{\alpha}_{t-1}} - \frac{(\mathbf{x}_t - \sqrt{\bar{\alpha}_t} \mathbf{x}_0)^2}{1-\bar{\alpha}_t} \big) \Big) \\
&= \exp\Big( -\frac{1}{2} \big( \color{red}{(\frac{\alpha_t}{\beta_t} + \frac{1}{1 - \bar{\alpha}_{t-1}})} \mathbf{x}_{t-1}^2 - \color{blue}{(\frac{2\sqrt{\alpha_t}}{\beta_t} \mathbf{x}_t + \frac{2\sqrt{\bar{\alpha}_{t-1}}}{1 - \bar{\alpha}_{t-1}} \mathbf{x}_0)} \mathbf{x}_{t-1} \color{black}{ + C(\mathbf{x}_t, \mathbf{x}_0) \big) \Big)}
\end{aligned}
$$
其中：
$$
\begin{align}
& p(\boldsymbol{x}_{t-1}|\boldsymbol{x}_0) = \sqrt{\bar{\alpha}_{t-1}} \mathbf{x}_0 + \sqrt{1-\bar{\alpha}_{t-1}} z \sim \mathcal{N} \left ( \sqrt{\bar{\alpha}_{t-1}}, 1-\bar{\alpha}_{t-1} \right ) \\
& p(\boldsymbol{x}_t|\boldsymbol{x}_0) = \sqrt{\bar{\alpha}_t} \mathbf{x}_0 + \sqrt{1-\bar{\alpha}_t} z \sim \mathcal{N} \left ( \sqrt{\bar{\alpha}_t}, 1-\bar{\alpha}_t \right ) \\
& p(\boldsymbol{x}_{t-1}|\boldsymbol{x}_0) = \sqrt{{\alpha}_{t-1}} \mathbf{x}_0 + \sqrt{1-{\alpha}_{t-1}} z \sim \mathcal{N} \left ( \sqrt{{\alpha}_{t-1}}, 1-{\alpha}_{t-1} \right )
\end{align}
$$
对于，我们知道它是服从正态分布的，因此，根据正态分布的公式：
$$
因此相对应的，可以得到$p(\boldsymbol{x}_{t-1}|\boldsymbol{x}_t, \boldsymbol{x}_0)$的方差$\tilde{\beta}_t$和均值$\tilde{\boldsymbol{\mu}}_t (\mathbf{x}_t, \mathbf{x}_0)$：
$$
\begin{aligned}
\tilde{\beta}_t 
&= 1/(\frac{\alpha_t}{\beta_t} + \frac{1}{1 - \bar{\alpha}_{t-1}}) 
= 1/(\frac{\alpha_t - \bar{\alpha}_t + \beta_t}{\beta_t(1 - \bar{\alpha}_{t-1})})
= \color{green}{\frac{1 - \bar{\alpha}_{t-1}}{1 - \bar{\alpha}_t} \cdot \beta_t} \\
\tilde{\boldsymbol{\mu}}_t (\mathbf{x}_t, \mathbf{x}_0)
&= (\frac{\sqrt{\alpha_t}}{\beta_t} \mathbf{x}_t + \frac{\sqrt{\bar{\alpha}_{t-1} }}{1 - \bar{\alpha}_{t-1}} \mathbf{x}_0)/(\frac{\alpha_t}{\beta_t} + \frac{1}{1 - \bar{\alpha}_{t-1}}) \\
&= (\frac{\sqrt{\alpha_t}}{\beta_t} \mathbf{x}_t + \frac{\sqrt{\bar{\alpha}_{t-1} }}{1 - \bar{\alpha}_{t-1}} \mathbf{x}_0) \color{green}{\frac{1 - \bar{\alpha}_{t-1}}{1 - \bar{\alpha}_t} \cdot \beta_t} \\
&= \frac{\sqrt{\alpha_t}(1 - \bar{\alpha}_{t-1})}{1 - \bar{\alpha}_t} \mathbf{x}_t + \frac{\sqrt{\bar{\alpha}_{t-1}}\beta_t}{1 - \bar{\alpha}_t} \mathbf{x}_0\\
\end{aligned}
$$


### [从DDPM到DDIM](https://kexue.fm/archives/9181)

DDPM是最基本的扩散模型。DDPM在生成（或者叫采样）的过程中仍然会加入随机值，因此即使有相同的输入也会有不同的输出。但是DDIM进行了简化，将随即部分省略，结果就是相同的输入模型会给出相同的输出。
这是扩散模型的采样结果：
$$\begin{aligned} 
p(\boldsymbol{x}_{t-1}|\boldsymbol{x}_t) \approx&\, p(\boldsymbol{x}_{t-1}|\boldsymbol{x}_t, \boldsymbol{x}_0=\bar{\boldsymbol{\mu}}(\boldsymbol{x}_t)) \\ 
=&\, \mathcal{N}\left(\boldsymbol{x}_{t-1}; \frac{1}{\alpha_t}\left(\boldsymbol{x}_t - \left(\bar{\beta}_t - \alpha_t\sqrt{\bar{\beta}_{t-1}^2 - \sigma_t^2}\right) \boldsymbol{\epsilon}_{\boldsymbol{\theta}}(\boldsymbol{x}_t, t)\right), \sigma_t^2 \boldsymbol{I}\right) 
\end{aligned}$$
 - 如果使$\sigma_t^2=0$，那么模型就变成了DDIM
 - 如果使$\sigma_t = \frac{\bar{\beta}_{t-1}\beta_t}{\bar{\beta}_t}$，那么模型就是DDPM

### [DPM-solver](https://www.bilibili.com/video/BV1B24y1Q7wZ/)

**这个还没看，有空看**

## 研究提案


Diffusion model是一个最近流行的机器学习模型，其目前广泛应用于图像生成、去噪、超分辨率等领域并取得了优异的效果。