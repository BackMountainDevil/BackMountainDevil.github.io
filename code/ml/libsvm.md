---
title: "libsvm 的 python 版使用办法"
date: 2021-08-06T21:56:09+08:00
lastmod: 2021-08-06T21:56:09+08:00
keywords: ['svm', 'machine learning', 'libsvm']
description: "记录 libsvm 的 python 使用办法。"
tags: ['svm']
categories: [机器学习]
author: "筱氚"
--- 
# 简介
记录 libsvm 的 python 使用办法。这里并不会介绍 svm 是什么以及实现原理。

## libsvm
在学习 SVM 的时候，代码用的是 sklearn 还是 tensorflow 的了，再次重温机器学习的时候在[支持向量机 兵王问题](https://www.icourse163.org/learn/ZJU-1206573810?tid=1464846443#/learn/content?type=detail&id=1243453160&sm=1)中听说了 [libsvm](https://www.csie.ntu.edu.tw/~cjlin/libsvm/)，它是台湾大学的 [Chih-Jen Lin](https://www.csie.ntu.edu.tw/~cjlin/)在 2000 年时候发布的 svm 工具包，现在（2021.8.6）最新版是 3.25 版本，提供了各大语言的版本（这老师是闲着没事干吗？是社区贡献的其它语言版本吗？还是研究生）。在其页面上还提供了[新手的入门指引（英文版）](https://www.csie.ntu.edu.tw/~cjlin/papers/guide/guide.pdf)

## 专有词汇表
SVM support vector machine 支持向量机  
SVMs Support Vector Machines 支持向量机  
penalty  parameter 惩罚系数（惩罚因子） C  s

# 一条龙
根据入门指引建议的步骤（Proposed Procedure）来进行学习。而不是自己瞎选内核瞎调参数测试然后选一个好看的。。。

## 安装
pip install -U libsvm-official

## 数据格式
将数据集转换为 liibsvm 所要求的格式，它大概长这个样子  
  > `<label> <index1>:<value1> <index2>:<value2> ... `   
  举个例子：`1 1:2.617300e+01 2:5.886700e+01 3:-1.894697e-01 4:1.251225e+02`  
  标签是 1,代表类别 1，有 1～4 个特征，每个特征的参数值分别是 2.617300e+01、5.886700e+01、-1.894697e-01、1.251225e+02


SVM 要求数据类型为数值型，如果是非数字型，需要进行类型转换。由于这里只是试水，因此下载[指引中所使用的数据集](https://www.csie.ntu.edu.tw/~cjlin/papers/guide/data/) - train.1、test.1（其它的也可以），因此这一步的转换就可以跳过了（下一步数据缩放也可以跳过了）。从 Table 1 可以知道第一个数据集的训练集有 3089 条数据，测试集有 4000 条数据，每条数据有 4 个特征，是一个二分类问题。上面数据中的例子就是对应训练集 train.1 的第一条数据。

## 数据缩放（Scaling）
对数据进行简单的缩放。又称归一化。这一步在使用 SVM 前是极其重要的一步。一方面是为了避免较大数值范围内的属性支配较小数值范围内的属；；另一个方面是降低计算过程中的复杂度，因为核值通常取决于特征向量的内积。一般推荐推荐的属性范围是 [−1,+1] 或者 [0,1]，需要注意的是，训练集和测试集的缩放系数应该保持一致。

## 核函数
svm 四种核函数，一般首先考虑选取通用的 rbf 核函数（默认采用的也是它，超参数比较少）。

### 参数选优 
指引中如此说道“需要注意的是，达到高的训练精度可能是没有用的”，毕竟测试集才是考试真题嘛。之前我选参数的时候是控制变量法，之修改我需要调整的参数，其它保持不动，调整到一个较好的分数，然后再调整下一个参数，这样做的问题会有一个明显的问题，就是我选出的都是好参数值，但结果并不是很理想。

那怎么去参数选优呢？手册中给出了一种可行的办法，采用交叉验证选择最佳参数 C 与 gamma 。相应的作者在[源码](http://www.csie.ntu.edu.tw/~cjlin/cgi-bin/libsvm.cgi?+http://www.csie.ntu.edu.tw/~cjlin/libsvm+tar.gz)下的 tools 提供了相应工具 grid.py。

解压源码之后需要编译一下来产生可执行文件，不然一会会报错“svm-train executable not found”。我使用的 Arch Linux 系统，进入目录下 make 一下搞定（make clean 可以卸载），然后把训练数据（train.1）拷贝到 libsvm-3.25/tools/ 下面，进入对应目录运行 `python grid.py  train.1`，之后就会自动调整参数，等一会出现结果，最后一行为 ，8.0 0.00048828125 96.8922，意识是这个程序尝试了 110 种 c 和 gama 的搭配（想起当初自己做实验一遍遍输入 c 和 g 来记录准确率），发现当 c=8.0 ， gama=0.00048828125 的时候在五折交叉校验中取得最高的准确率。对比了一下，使用默认参数的准确率只有 67%，使用上面的参数的准确率是 97%。

之后就是训练模型，再用获取的模型进行测试和预测了，完整代码如下

```python
# 引入 libsvm 工具包
from libsvm.svmutil import (svm_parameter, svm_predict, svm_problem,
                            svm_read_problem, svm_save_model, svm_train)
y, x = svm_read_problem('train.1')  # 读取训练集的数据
prob = svm_problem(y, x)
param = svm_parameter('-c 8.0 -g 0.00048828125')  # 根据之前的结果设置参数
# model = svm_train(y, x)  # 使用默认参数训练集的数据训练模型
# model = svm_load_model('model_svm') # 读取文件中的模型
model = svm_train(prob, param)  # 使用指定参数训练
svm_save_model('model_svm', model)  # 将训练好的模型保存到文件 model_svm 中

print('Train:')
p_label, p_acc, p_val = svm_predict(y, x, model)
# print("p_label: ", p_label)   # 每个数据对应的预测类别（标签）
print("p_acc: ", p_acc)  # 预测的精确度，均值和回归的平方相关系数
# print("p_val: ", p_val)   # 在指定参数'-b 1'时将返回判定系数(判定的可靠程度)

print('Test:')
yt, xt = svm_read_problem('test.1')  # 读取测试集的数据
p_label, p_acc, p_val = svm_predict(yt, xt, model)
```

## 分析
### 输出分析
一路走下来，结果如下所示
```bash
optimization finished, #iter = 1713 # 迭代次数
nu = 0.071714 # 核函数类型的参数
obj = -1658.282958, rho = -0.626561 # 判决函数的偏置项b
nSV = 326, nBSV = 198
Total nSV = 326 # 支持向量总个数
Train:
Accuracy = 97.2483% (3004/3089) (classification) # 训练集准确率
p_acc:  (97.24830042084817, 0.02751699579151829, 0.8830128932087852)
Test:
Accuracy = 97% (3880/4000) (classification) # 测试集准确率
```

### 模型文件分析
```txt
// file：  model_svm 
svm_type c_svc # svm类型，默认为c_svc
kernel_type rbf # 核函数类型，默认为 rbf 核
gamma 0.00048828125 # rbf 核的参数 gama 的值
nr_class 2  # 分几类
total_sv 326  # 支持向量总个数
rho -0.62656057185873659  # 判决函数的偏置项b
label 1 0 # 原始文件中的类别标识
nr_sv 197 129 # 每个类的支持向量机的数量
SV  # 下面就是全部的支持向量和对应权重（计数格式和训练集的有点区别，而且没有标签，稍微改一下可以在训练集里找到对于的数据的标签了）
```

### 优劣分析
分析模型能只看准确率吗？媒体报到也是如此，因为门外汉也只能看到这一步，但是多高的准确率才能说明问题呢？一是打比赛看排名，二是看混淆矩阵对比瞎猜概率。

准确率、召回率、ROC 等等，但是 libsvm 没有提供这个东西。。。


## 卸载
pip install -U libsvm-official

# 总结
libsvm 也提供了傻瓜版的教程，让它自己调参自己画图，这个工具就是 tools 下面的 easy.py,使用它还额外需要安装 gnuplot，在 Arch 上可以快速通过 AUR （yay -S gunplot）安装。

```bash
$ python easy.py train.1 test.1 
Scaling training data...
Cross validation...
Best c=2.0, g=2.0 CV rate=96.9893
Training...
Output model: train.1.model
Scaling testing data...
Testing...
Accuracy = 96.875% (3875/4000) (classification)
Output prediction: test.1.predict
```
通过和上面的 grid.py 得到的超参数的结果不一样，准确率略微低那么一丢丢，难道里面调用的参数选优不一样？？我又运行了几次 grid，发现得到的结果依旧是 8.0 0.00048828125 96.8922，这是为什么呢？今天到此为止吧。

# 参考
- [支持向量机 兵王问题](https://www.icourse163.org/learn/ZJU-1206573810?tid=1464846443#/learn/content?type=detail&id=1243453160&sm=1)
- [libsvm](https://www.csie.ntu.edu.tw/~cjlin/libsvm/)
- [Chih-Jen Lin's Home Page](https://www.csie.ntu.edu.tw/~cjlin/)
- [LibSVM for Python 使用。 finley。  ](https://www.cnblogs.com/Finley/p/5329417.html)：参数中文解释
- [LIBSVM使用指南。Timmy_Y 2016-12-17](https://blog.csdn.net/mingtian715/article/details/53712318)
- [LibSVM学习详细说明。 做一个勤劳的孩子。 2016-09-22](https://www.cnblogs.com/-ldzwzj-1991/p/5897199.html)：model 文件解析
  >  tools——主要包含四个python文件，用来数据集抽样(subset)，参数优选（grid），集成测试(easy),数据检查(checkdata)；