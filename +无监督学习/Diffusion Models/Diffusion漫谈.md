## [Diffusion漫谈](https://kexue.fm/archives/9164)

可以看这一整个系列，介绍了很多diffusion的概念

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