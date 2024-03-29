
# survey/overview/review

## paper
- NAS一篇综述，从Search Space，search strategy performance论述NAS.

  - [Neural Architecture Search: A Survey](https://arxiv.org/pdf/1808.05377.pdf)


- 香港中文大学，第四范式综述。

  - [Taking the Human out of Learning Applications:A Survey on Automated Machine Learning](https://arxiv.org/pdf/1810.13306.pdf)[2018.10]

- 机器学习的survey，和Neural Architecture Search不相关。

  - [Survey on Automated Machine Learning](https://arxiv.org/pdf/1904.12054.pdf)

- IBM Martin团队对NAS总结。

  - [A Survey on Neural Architecture Search](https://arxiv.org/pdf/1905.01392.pdf)
  
- [Automated Machine Learning: State-of-The-Art and Open Challenges](https://arxiv.org/pdf/1906.02287.pdf)


## awesome

- [D-X-Y/Awesome-NAS](https://github.com/D-X-Y/Awesome-NAS)
- [awesome-automl-papers](https://github.com/hibayesian/awesome-automl-papers)
- https://github.com/datasystemsgrouput/automl_survey


## Blogs
- [What’s the deal with Neural Architecture Search?](https://determined.ai/blog/neural-architecture-search/)
- [Google Could AutoML](https://cloud.google.com/vision/automl/docs/beginners-guide)
- [PocketFlow](https://pocketflow.github.io/)
- [AutoML Challenge](http://automl.chalearn.org/)
- [AutoDL Challenge](https://autodl.chalearn.org/)
- Auto is the new black — Google AutoML, Microsoft Automated ML, AutoKeras and auto-sklearn
  - https://medium.com/@santiagof/auto-is-the-new-black-google-automl-microsoft-automated-ml-autokeras-and-auto-sklearn-80d1d3c3005c
- Illustrated: Efficient Neural Architecture Search
  - https://towardsdatascience.com/illustrated-efficient-neural-architecture-search-5f7387f9fb6
- [Illustrated: Efficient Neural Architecture Search](https://towardsdatascience.com/illustrated-efficient-neural-architecture-search-5f7387f9fb6)

  
## classifier

- 2017 ICLR论文。google 首次尝试使用NAS构造CNN模型，基于RNN和强化学习的思路，训练和测试集CIFAR-10。RNN作为控制器，生成变长字符串，控制child network网络模型的连接。
child network在验证数据集反馈准确率作为reward信息，计算策略的梯度更新控制器。重复以上过程，控制器将学习如何随着时间的推移改进其搜索。
论文同时提出固定搜索空间(滤波器宽度/高度[1, 3, 5, 7]，滤波器个数[24,36,48,64]),增加跳跃连接等设计方法。
  
  google是强化学习的引领者，基于强化学习生成神经网络模型NAS-CNN和NAS-RNN两个架构，虽然数据集仅为CIFAR-10,作为行业开创者使自动生成网络模型成为可能。另外使用800块GPU,
  一般实验室即使有类似idea也难以实施。

  - [Neural Architecture Search with Reinforcement Learning](https://arxiv.org/pdf/1611.01578.pdf)[2016.11]

- 继NAS之后，Google又一力作NASNet。论文提出proxy dataset CIFAR-10，通过堆叠训练的Normal Cell和Reduction Cell，生成在ImageNet classification、mobile network和COCO Object detection数据集，
state-of-art性能。论文实验部分对比Random search网络结构方式有显著优势。

  特点：固定cell,每个cell内部学习5个block operation，cnn可以由同构cell进行堆叠而构成。计算量大，500 GPU*4days。结构不规则，特征由RNN学习。

  -- [Learning Transferable Architectures for Scalable Image Recognition](https://arxiv.org/pdf/1707.07012.pdf)[2017.07]


- Google 团队提出MnasNet，使用强化学习的思路，提出基于移动终端的CNN模型的神经网络自动搜索方法。论文将实时性结合到主要搜索目标中，
基于强化学习，将实时性和准确率作为 reward，并且直接使用手机平台（Pixel phones 等）运行模型，测量实时性和准确率，另外根据Params更新A1-A3模型。
论文还提出一种新的分解层次搜索空间，既每个block单独搜索。在ImageNet(MnasNet)以及COCO(MnasNet-A1 + SSDLite)均取得state-of-art水平，（相比mobileNetv2 
，速度和准确率均超越）。在搜索空间生成8k个模型，只有15个模型在ImageNet完整训练，1个模型在COCO训练。

  - [MnasNet: Platform-Aware Neural Architecture Search for Mobile](https://arxiv.org/pdf/1807.11626.pdf)[2018.07]
  
- Google提出，ICML2018论文。不太了解NAS基于梯度优化的原理，所以追溯相关论文加深理解。论文首先构架one-shot model（FBNet提到的super net），训练包含搜索空间的所有卷积路径。
评估时随机zero out r^(1/k)个分支，并评估舍弃对网络性能影响较小的分支，Re-train the most promising architectures。论文中drop也是按照一定策略进行。论文基于KL divergence
论证less/most important operations假设的合理性。#todolist:机器学习kl理论基础，GumbelSoftmax为什么可代替random dropout.

  - [Understanding and Simplifying One-Shot Architecture Search](http://proceedings.mlr.press/v80/bender18a/bender18a.pdf)

- NAS一般是依据人类设计的CNN构造cell,堆叠cell单元。Facebook Ross Girshick，Kaiming He等设计一个基于图论的网络生成器生成随机网络。
实验效果在RandWire-WS数据集，RandWire-WS相比MobileNet v2，Amoeba-C没有太大提升，在COCO目标检测数据集相比ResNeXt-50和ResNeXt-101，
在FLOPs计算量相同情况下，最高有1.7%的提升。为保证公平，论文的随机网络生成器只迭代250 epoch，如果迭代更高的epoch是不是可以生成准确率更高
计算更快的网络模型？
  - 缺点：每个随机生成的网络都需要在完整数据集训练，论文没有说明使用GPU数量和天数

  - [Exploring Randomly Wired Neural Networks for Image Recognition](https://arxiv.org/pdf/1904.01569.pdf)[2019.04]

- ICLR2019论文，CMU和goole提出的DARTS(Differentiable ARchiTecture Search)模型,在连续域进行模型搜索，基于梯度下降方式的方式优化模型（NAS梯度算法的首次提出者？），
通过以可微的方式进行神经网络结构搜索，用于解决NAS搜索空间大及占用GPU资源问题。优化目标是w和a，提出近似迭代优化：w和a分别在权重和架构空间的梯度下降布置之间交替优化。
【扩展阅读】连续域结构搜索

  - [DARTS: Differentiable Architecture Search](https://arxiv.org/pdf/1806.09055.pdf)[2018.06]
  
- ICLR2019论文，麻省理工学院提出ProxylessNAS。NAS的巨大搜索空间和proxy tasks代理训练，影响NAS的进一步发展。DARTS基于梯度下降解决了搜索空间问题，本文重点在解决proxy tasks，
既直接学习目标训练集和目标硬件平台的网络架构（为不同数据集，不同硬件平台定制学习不同的网络架构？）。
1、Proxyless。论文首先构建super network（包括所有分支的搜索空间），把 NAS 当作一个 Path level pruning 问题，训练后
pruning冗余的路径。为了解决显存占用和搜索空间线性增长的问题，将路径上的arch parameter二值化（todolist：复习BinaryConnect），在搜索时仅有一条路径处于激活状态，将显存复杂度从O(n)降低到O(1)。
2、latency。论文引入基于latency estimation model的损失函数，可以通过梯度优化的方法建模。基于Monte-Carlo的REINFORCE 训练binarized weights。
论文对比搜索架构实现的GPU/CPU/mobile model，提出几个模型设计准则：GPU prefers shallow and wide model with early pooling; CPU prefers deep and narrow model with late pooling. 
Pooling layers prefer large and wide kernel. Early layers prefer small kernel. Late layers prefer large kernel.
论文从pruning角度解决NAS搜索问题，将NAS和compression连接在一起，compression领域和RL领域很多算法都可以应用在NAS。

  -- [ProxylessNAS: Direct Neural Architecture Search on Target Task and Hardware](https://arxiv.org/pdf/1812.00332.pdf)[2018.12]

- arxiv论文，同济大学和华为诺亚联合提出P-DARTS，基于DARTS基础上改进的NAS方法。论文认为DARTS的搜索和评估架构不一致(depth gaps),导致学习能力降低(貌似performance没有较大提升)。提出策略：
在搜索过程中渐进式增加网络深度，使得搜索网络深度和评估网络深度一致。

  缺点：基于梯度下降的算法在搜索时间上显著降低(0.3 GPU days)，但是在准确率和实时性貌似没有质的改进(1%以内的提升很难说是训练策略的提升)，话说增加/或减少GPU搜索与训练时间，target是performance。

  - [Progressive Differentiable Architecture Search:Bridging the Depth Gap between Search and Evaluation](https://arxiv.org/pdf/1904.12760.pdf)[2019.04]
  
- CVPR2019论文，加州大学伯克利分校,普林斯顿大学和Facebook联合提出一种NAS搜索方法。首先定义网络的框架结构和9种layer-wise的搜索空间，定义Latency-Aware Loss Function∝cross-entropy &Latency,
cross-entropy计算精度，Latency基于速查表（预先计算9种layer-wise计算数据）。Stochastic super net是每一层都让9中架构并联，基于可微分的GumbelSoftmax训练整个super net,根据训练结果Pθ设计网络架构。
这种可微分方式比RL方式快速很多。

 - [FBNet: Hardware-Aware Efficient ConvNet Design via Differentiable Neural Architecture Search](https://arxiv.org/pdf/1812.03443.pdf)[2018.12]

- CVPR2019论文(oral)，悉尼科技大学和百度联合提出GDAS(Differentiable Architecture Sampler)。搜索空间是基于FBNet提出的Stochastic super net，梯度运算同样基于GumbelSoftmax。论文改进在训练方式：首先CIFAR训练，选择normal cell用于ImageNet网络设计。
normal cell输入为two previous cells。Reduction Cell是人工设计。论文的加速设计是基于hij(one-hot vector),既计算BP时只有一个支路。轮设计的GDAS (FRC) 在V100 GPU仅运行4个小时，远远高于state-of-art
的搜索效率。

  - [Searching for A Robust Neural Architecture in Four GPU Hours](https://raw.githubusercontent.com/D-X-Y/GDAS/master/data/GDAS.pdf)
  
- google最新设计MobileNetv3,应用于移动端CPU。网络的架构基于NAS实现的MnasNet，根据图像分辨率，准确率和实时性设计两个模型MobileNetV3-Large和MobileNetV3-Small。在分类，目标检测和语义分割验证performance。
MobileNetv3用到的tricks:
  1、backbone基于MobileNetV2 + Squeeze-and-Excite(扩张系数1/4)。SENet继残差设计之后，成为神经网络另外一个标配。
  2、复用MnasNet-A1框架。MnasNet-A1使用64 TPUv2*4.5days，采样 8K 模型，精选top15在ImageNet模型训练。论文中对MnasNet LAT权重系数w改进。tensorflow已开源MnasNet(https://github.com/tensorflow/tpu/tree/master/models/official/mnasnet),github也有pytorch keras等实现。
  3、NetAdapt也是google提出，在约束latency条件下，序列方式微调每一层的网络结构，暂时不了解细节。
  4、Redesigning Expensive Layers。对earlier layers和last layers裁剪，initial  banks的32个滤波器减少到16个滤波器。
  5、Nonlinearities。论文部分block用h-swish{x*ReLU6(x + 3)/6}代替swish{x*sigmoid(x)}。swish和h-swish在部署时没有准确率差别，但是更容易实现软件和硬件的优化，量化。论文模型在deeper层使用h-swish。
  6、语义分割的R-ASPP模块，是由ASPP(Atrous Spatial Pyramid Pooling)改进，主要包括1*1 卷积和avg-pooling实现。
  MobileNetV3使用tricks较多，但是模型实时性和准确率都是state-of-art，设计过程复杂，使用简单有效。

  - [Searching for MobileNetV3](https://arxiv.org/pdf/1905.02244.pdf)[2019.05]
  
  
- 卡内基·梅隆大学，微软，哈工大联合提出，基于differentiable NAS的Single-Path NAS策略(共享卷积核参数，降低计算资源)，backbone类似于MobileNetv2设计，搜索空间包括SEnet,3x3,5x5卷积,可分离卷积，Skip ops等。
  24 TPU-hours实现架构搜索，Top 1性能优于MobileNet v3,推断延时80ms(Pixel 1)，在人类先验知识基础上完全是机器设计。轮最后提出的NAS hyperparameter optimization问题不理解。 

  - [Single-Path Mobile AutoML: Efficient ConvNet Design and NAS Hyperparameter Optimization](https://arxiv.org/pdf/1907.00959.pdf)[2019.07]

- 小米团队楚洋洋，张勃等新作。

  - [FairNAS: Rethinking Evaluation Fairness of Weight Sharing Neural Architecture Search](https://arxiv.org/pdf/1907.01845.pdf)[2019.07]


- 
  - [EfficientNet: Rethinking Model Scaling for Convolutional Neural Networks](https://arxiv.org/pdf/1905.11946.pdf)[2019.05]
## Detection

- 中科院自动化所和旷视联合提出，Object Detection with FPN on COCO优于ResNet101,但是FLOPs比ResNet50低。基于ShuffleNetV2的架构也有较好的表现。

  - [DetNAS: Neural Architecture Search on Object Detection](https://arxiv.org/pdf/1903.10979v1.pdf)[2019.03]

- Google基于AutoML提出Detection模型，基于RetinaNet网络，解决FPN多尺度金字塔问题。通过Neural Architecture Search搜索各种类型的
top-down,bottom-up特征层的连接方式（还是连连看），取得state-of-art的mAP同时降低推断时间。100 TPU的模型也不是可轻易实现。

  - [NAS-FPN: Learning Scalable Feature Pyramid Architecture for Object Detection](https://arxiv.org/pdf/1904.07392.pdf)[2019.04]
  
 - 西工大提出，基于one-stage object detector (FCOS)的基础，基本运算包括可分离卷积，空洞卷积，可变形卷积，搜索空间包括FPN，Prediction Head和Head Weight Sharing。论文第一句话说的好：The success of deep neural networks relies on significant architecture engineering，现在Deep learning就是在各种架构上作文章。论文的trick使用比NAS-FPN多，但是性能仅仅相比one-stage模型提升1%，相比two-stage还是有较大差距，说明搜索架构的backbone还很重要，关键有一个较优的先验知识。论文虽然在搜索时间上有优势（强化学习30GPU days可完成？存疑），性能上却没有优势。
 
  - [NAS-FCOS: Fast Neural Architecture Search for Object Detection](https://arxiv.org/pdf/1906.04423.pdf)[2019.06]

## Recognition

- 华为等提出的人脸识别模型，在MS-Celeb-1M和LFW数据集state-of-art。主流人脸识别模型集中于度量学习（Metric Learning）和分类损失函数函数改进（Cross-Entropy Loss，Angular-Softmax Loss，
Additive Margin Softmax LossArcFace Loss等）。论文基于强化学习的NAS设计，network size 和latency作为reward(论文的实验没有对比测试latency或者模型尺寸)，仅说明最小网络参数NASC 16M。
这是NAS在人脸识别的首测尝试，分类，检测，识别都有涉及，图像分割应该不远。

  - [Neural Architecture Search for Deep Face Recognition](https://arxiv.org/pdf/1904.09523.pdf)[2019.04]

## Semantic  Segmentation

- CVPR2019论文，驭势科技，新加坡国立大学等联合提出轻量型分类和语义分割网络，成为"东风"网络，主要解决Speed/Accuracy trade-off问题，性能接近于deeplab v2但是速度>50 fps。
论文的backbone基于典型residual block，但是提出Acc(x)和Lat(x)用于评价准确率和推断时间，节省神经网络的随机搜索。另外没有选用RNN/LSTM,而是Random select an architecture。
论文提出的算法，训练200个模型架构时已经去除438个模型架构，每个模型训练5-7个小时(8卡机)。训练200个架构月400GPU/days。
模型在嵌入式硬件TX2和1080Ti均有评测，准确率和性能均有明显优势。去年有项目曾使用ENet/ICNet，速率尚可准确率不如人意，也许"东风"系列有明显改善。

[Partial Order Pruning: for Best Speed/Accuracy Trade-off in Neural Architecture Search](https://arxiv.org/pdf/1903.03777.pdf)[2019.03]

- 李飞飞团队作品。基于DeepLab系列框架搜索。

  - [Auto-DeepLab:Hierarchical Neural Architecture Search for Semantic Image Segmentation](https://arxiv.org/pdf/1901.02985.pdf)[2019.01]





## Graph CNN

- 2019ICLR论文，Uber等联合提出，基于Graph CNN实现的NAS,性能虽然没有太惊艳，但是基于Graph CNN应该有更广阔用处。

  - [Graph HyperNetworks for Neural Architecture Search](https://arxiv.org/pdf/1810.05749.pdf)[2018.10]


## Architecture

- facebook开源框架，基于MCTS和DNN,解决分类，目标检测，风格迁移，图像描述4个任务。

  - [AlphaX: eXploring Neural Architectures with Deep Neural Networks and Monte Carlo Tree Search](https://arxiv.org/pdf/1903.11059.pdf)[2019.03]


## Benchmark on ImageNet

| Architecture       | Top-1 (%) | Top-5 (%) | Params (M) | +x (M) | GPU | Search cost |
| --- | --- | --- | --- | --- | ---   | ---    |
| [Inception-v1](https://arxiv.org/pdf/1409.4842.pdf)       | 30.2      | 10.1      | 6.6        | 1448   | -   | -    |
| [MobileNet-v1](https://arxiv.org/abs/1704.04861)       | 29.4      | 10.5      | 4.2        | 569    | -   | -    |
| [ShuffleNet](http://openaccess.thecvf.com/content_cvpr_2018/CameraReady/0642.pdf)         | 26.3      | -         | ~5         | 524    | -   | -    |
| [MobileNetV2](https://arxiv.org/pdf/1801.04381.pdf)     |28.0          |  -       | 3.4M  | 300M  | - | - |
| MobileNetV2-1.4 |25.3          |  -       |6.9M   | 585M  | - | - |
| [MobileNetV3](https://arxiv.org/pdf/1905.02244.pdf)     | 24.8 |  - | 5.4M | 219M | -| -|
| [NASNet-A]((http://openaccess.thecvf.com/content_cvpr_2018/papers/Zoph_Learning_Transferable_Architectures_CVPR_2018_paper.pdf))           | 26.0      | 8.4       | 5.3        | 564    | 450 | 3-4  |
| NASNet-B           | 27.2      | 8.7       | 5.3        | 488    | 450 | 3-4  |
| NASNet-C           | 27.5      | 9.0       | 4.9        | 558    | 450 | 3-4  |
| [AmobebaNet-A](https://arxiv.org/pdf/1802.01548.pdf)       | 25.5      | 8.0       | 5.1        | 555    | 450 |  7   |
| AmobebaNet-B       | 26.0      | 8.5       | 5.3        | 555    | 450 |  7   |
| AmobebaNet-C       | 24.3      | 7.6       | 6.4        | 555    | 450 |  7   |
| [Progressive NAS](http://openaccess.thecvf.com/content_ECCV_2018/papers/Chenxi_Liu_Progressive_Neural_Architecture_ECCV_2018_paper.pdf)    | 25.8      | 8.1       | 5.1        | 588    | 100 | 1.5  |
| [DARTS-V2](https://arxiv.org/abs/1806.09055)           | 26.9      | 9.0       | 4.9        | 595    |  1  |  1   |
| [GDAS](https://raw.githubusercontent.com/D-X-Y/GDAS/master/data/GDAS.pdf) | 26.0      | 8.5       | 5.3        | 581    |  1  |  0.21   |
| [RandWire-WS](https://arxiv.org/pdf/1904.01569.pdf)        | 25.3±0.25 | 7.8       | 5.6±1      |583±6.2 |  -  |   -     |
| [EfficientNet](https://arxiv.org/pdf/1905.11946.pdf) | 15.4% | 2.9%  | 66 | - | -|-| 


Auto is the new black — Google AutoML, Microsoft Automated ML, AutoKeras and auto-sklearn

https://medium.com/@santiagof/auto-is-the-new-black-google-automl-microsoft-automated-ml-autokeras-and-auto-sklearn-80d1d3c3005c



Neural architecture search has been applied so far to 
(1) convnet-based image classifiers, (2) recurrent neural networks, 
(3) activation functions, (4) SGD optimizers, (5) data augmentation, 
(6) transformer, (7) object detection and so on.