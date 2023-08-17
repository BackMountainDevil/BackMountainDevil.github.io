遗传算法、粒子群算法

进阶神经网络：玻尔兹曼机，受限玻尔兹曼机，递归神经网络等

深度神经网络：深度置信网络，卷积神经网络，深度残差网络，LSTM网络等

# mp 1957
感知器（感知机）算法，首次模仿神经元的数学模型。首先提出了机器学习的框架。
占用内存小，每次只处理一部分数据

Marvin Minsky 1969 在 《Perceptrons》（《感知器》）明确定义线性可分和非线性可分，并指出感知器只能解决线性可分问题，并用数学论证实际生活中的分类问题大都是线性不可分，

单层感知器：只有一层神经元、输出结果仅两种；无法解决异或问题

线性神经网络： 加入梯度调整、输出可以是任意值（两个激活函数）

BP(Error Back Propagtion)神经网络：

Hopfield神经网络：从输出到输入有反馈连接

Lenna 图像

过拟合和欠拟合

# 卷积神经网络 CNN

如果层与层的非线性函数采用阶跃函数，那么三层神经网络可以模拟任意的非线性函数。

LeNet 1989. Yann Lecun. 首次、CNN. 手写数字识别

AlexNet 2012. Alex Krizhevsky. 较大的卷积核 CNN. ImageNet 冠军. ReLU、Dropout

GoogLeNet 2014. CNN. ImageNet 冠军. inception

VGG 2014. 小卷积核 CNN. ImageNet 亚军.

ResNet 2015. Kaiming He. CNN. shortcut 残差

DenseNet 2017. Gao Huang. CNN.

# 循环神经网络 RNN

解决 cnn 前后数据输入没有关联的问题

LSTM 1997

# 对抗式生成网络 GANs

# 强化学习 RL

# SVM 1995
线性可分 线性不可分
同时计算全部数据求解全局最优解，计算量大，适合少量数据。

最大间隔 支持向量  

核函数：低维度到高维度
Linear  线性内核
Poly  多项式内核
Rbf 高斯径向基函数核（不懂就选这个） 
Tanh  sigmoid核


libsvm工具包
兵王问题：分类问题
训练集 和 测试集

交叉验证：折数越多，训练时间越大，几折就训练几个模型
五折交叉验证
数据集平分为 ABCDE
ABCD-E
ABCE-D
ABDE-C
ACDE-B
BCDE-A

数据扩增

单纯用识别率来判断系统好坏是没意义的
混淆矩阵（可大体计算瞎猜的概率）
TP FN
TN FP

ROC 曲线 越靠近左上角越好
AUC 曲线 右下方面积越大越好
EER 曲线 等错误率

# AI 应用

自然语言处理 (NLP)：语音识别、语言翻译、语音助手、机器人客服

计算机视觉：照片标记、医疗成像、自动驾驶汽车

文本生成/对话

[mlc-llm 陈天奇团队](https://github.com/mlc-ai/mlc-llm):在本地跑语言模型，无需服务器支持，并通过手机和笔记本电脑上的本地GPU进行加速。手机，笔记本均可

[TigerBot 陈烨团队](https://github.com/TigerResearch/TigerBot):语言任务大模型，支持插件联网

多模态

[吴恩达大模型系列课程中文版 - datawhalechina](https://github.com/datawhalechina/prompt-engineering-for-developers)


文转图 Stable Diffusion
https://zhuanlan.zhihu.com/p/563073449

文转视频

https://videocomposer.github.io

文转音乐

https://about.fb.com/news/2023/08/audiocraft-generative-ai-for-music-and-audio/

Multi-Modal Chat https://github.com/silverriver/MMChat

https://github.com/FreedomIntelligence/LLMZoo


# 参考

[微软人工智能教育与学习共建社区](https://microsoft.github.io/ai-edu/index.html)

[IBM 云计算知识学习 什么是机器学习？](https://www.ibm.com/cn-zh/cloud/learn/machine-learning)

[easyAI](https://easyai.tech/)

[hanbingtao 深度学习入门](https://zybuluo.com/hanbingtao/note/433855)

[shuzy 基础神经网络文章](https://www.zhihu.com/people/james_cu/posts)
