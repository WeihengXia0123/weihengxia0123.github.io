---
title: "Udacity Pytorch Notes"
date: 2020-08-16T13:27:58+02:00
categories:
- category
- subcategory
tags:
- tag1
- tag2
keywords:
- tech
thumbnailImage: img/udacity.png
---

<!--more-->
### manual_seed() 用法
```python
torch.manual_seed(100) 
test = torch.rand(1,4)
print(test)
```
如果seed里面的value一致，那么无论多少次rand，得到的随即数字都是一样的。也就是说，这个seed就是一个id，同样的id，随即出来的数大小，无论重复随即多少次，都是一样的。

### torch.mm和vector直接*的不同
```python
h = activation(torch.sum(img_flat*W1)+B1)
```
本来按上面这么写的，结果发现报错说Size不一致。我就看了看shape，对的呀？咋回事？
原来是用了*，而不是matrix_multiplication！
下图为正解：
```python
h = activation(torch.mm(img_flat,W1)+B1)
```

### Flatten images
```python
# Flatten the input images
inputs = images.view(images.shape[0],-1)

img_flat = torch.flatten(images, start_dim=1)
```

### Softmax
首先，torch.sum(xxxx, dim=1)，如果dim=0那么就会对column进行sum，如果dim=1就会对row进行sum。
其次，shape=[64]是错误的，因为这样“because PyTorch will try to do the division across the columns”，因此我们要64张图片进行除法，就需要64行, [64,1]
```python
def softmax(x):
    return torch.exp(x)/torch.sum(torch.exp(x), dim=1).view(-1, 1)
```
### nn.ReLU()和F.relu()的不同
参考知乎[用户糖吃可好](https://www.zhihu.com/question/66782101/answer/579393790)的问答。

- 两者的调用方式不同:
`nn.Xxx` 需要先实例化并传入参数，然后以函数调用的方式调用实例化的对象并传入输入数据。
```python
inputs = torch.rand(64, 3, 244, 244)
conv = nn.Conv2d(in_channels=3, out_channels=64, kernel_size=3, padding=1)
out = conv(inputs)
```
`nn.functional.xxx`需要同时传入输入数据和weight, bias等其他参数 。
```python
weight = torch.rand(64,3,3,3)
bias = torch.rand(64) 
out = nn.functional.conv2d(inputs, weight, bias, padding=1)
```
- `nn.Xxx`继承于`nn.Module`， 能够很好的与`nn.Sequential`结合使用， 而`nn.functional.xxx`无法与`nn.Sequential`结合使用。
- `nn.Xxx`不需要你自己定义和管理`weight`；而`nn.functional.xxx`需要你自己定义`weight`，每次调用的时候都需要手动传入`weight`, 不利于代码复用。
- 上面两种定义方式得到CNN功能都是相同的，至于喜欢哪一种方式，是个人口味问题，但PyTorch官方推荐：具有学习参数的（例如，conv2d, linear, batch_norm)采用nn.Xxx方式，没有学习参数的（例如，maxpool, loss func, activation func）等根据个人选择使用nn.functional.xxx或者nn.Xxx方式。但关于dropout，个人强烈推荐使用nn.Xxx方式，因为一般情况下只有训练阶段才进行dropout，在eval阶段都不会进行dropout。使用nn.Xxx方式定义dropout，在调用model.eval()之后，model中所有的dropout layer都关闭，但以nn.function.dropout方式定义dropout，在调用model.eval()之后并不能关闭dropout。

### nn.CrossEntropyLoss()
It combines `nn.LogSoftmax()` and `nn.NLLLoss()` together

### nn.MaxPool2d(2,2)
