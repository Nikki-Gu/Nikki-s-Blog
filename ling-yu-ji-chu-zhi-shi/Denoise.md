# 图像去噪

目前高斯去噪工作主要分类：

- 有监督（blind、unblind）

- 无监督

  以N2N为基础，不断降低训练难度，从多张图片到单张图片进行去噪

  - N2N
    - 需要同一场景多个噪声图像
  - N2V(Noise to void)
    - 盲点网络
    - 要求噪声与信号无关；存在丢失信息的问题
  - S2S（self 2 self）
    - 解码端添加dropout，求均值
  - Noiser2noise
    - 两倍噪声图像后的期望关系
  - NBR2NBR

- 对抗生成