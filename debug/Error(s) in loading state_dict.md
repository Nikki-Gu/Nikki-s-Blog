# 多GPU训练，测试使用单GPU

加载模型运行报错如下：

```RuntimeError: Error(s) in loading state_dict for Swintransformer: Missing key(s) in state_dict: “conv1.0...```



**多GPU训练，测试使用单GPU**

解决方法：需要使用多GPU进行测试，在加载权重前使用`model = torch.nn.DataParallel(model)`将模型移动到多GPU上。

**多GPU训练，测试使用CPU**

解决方法：加载权重时添加参数map_location='cpu'