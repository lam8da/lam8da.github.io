---
layout: post
title: 深度学习的一些笔记
categories:
    - 人工智能
tags:
    - 图像处理
    - 深度学习
    - Work in Progress
---

{::comment} Not show line_numbers {:/comment}
{::options syntax_highlighter_opts="{default_lang: c++ \}" /}

* TOC
{:toc}

# Convolutional Neural Networks (by Andrew Ng)

- 视频链接在[这里](https://www.youtube.com/playlist?list=PLkDaE6sCZn6Gl29AoE31iwdVwSG-KnDzF)
- 例子：边缘检测：通过用conv2d并设置特定的filter/kernel的值可以用该filter/kernel做边缘检测（横的或者竖的或者斜的或其他）
- LeNet：最原始用于手写数字识别的网络，只有几层
- AlexNet: Hinton等人发明的识别一般图像的网络，有大约十来层？感觉每一层都是hand-tuned的
- VGG-16：避免hand-tuned：统一采用相同规格的block例如3x3的conv2d，证明也能达到好的效果
- ResNet: 增加了short-cut，可以使网络通过学习自动选取究竟是否应用新的一层。例如如果新的层会导致精度更差，训练过程能够学习到这个结果从而通过自动调参来增大short-cut的权重从而降低其认为影响了精度的block的权重（让该block变为大约相当于identity的功能，从而大程度上禁止了该block的功能）。而普通的deep network没有这种选择所以有可能会在很deep的时候反而精度更差。
- Inception: 与其决定每个block用1x1conv还是3x3conv还是别的，干脆全都用然后将结果concat起来作为下一层输入。block内用到的层有：1x1conv+3x3conv，1x1conv+5x5conv，1x1conv，maxpool等。
- Transfer learning
  - 可以下载已经train好的如ResNet50的model，然后把最后的softmax层替换掉，将其他层的参数freeze，只重新train最后softmax层的参数（用新的数据）
  - 或者可以任意选择freeze前面N层而只重新train后面那些层（当然还是要先把softmax换为适合新应用的类别个数的softmax）
  - 可以预处理：把所有新数据的前N层的结果预先求出，避免之后重复计算
- 数据预处理/增强技术：mirroring，random cropping，rotation，color shifting，等
- Object Detection
  - 基本思想：slicing window
  - 如何避免真正的slicing window并为每个window算一遍是否里面有某个object：直接用conv处理原图，filter的strides设为slicing window的stride
  - anchor boxes和Yolo algorithm：没太懂？
  - R-CNN vs. Fast R-CNN vs. Faster R-CNN
- 人脸识别
  - Triplet loss：每次的输入是一个triple：一个人脸图，一个和第一个图是同一个人的人脸图，一个其他人的人脸图。目标是最大化(前两图的相似性-第一和第三张图的相似性)。缺点是：需要显式定义相似性度量函数例如用余弦相似性，而这很可能不是最好的度量
  - 另一种种方法是，把相似性度量改为一个network module（黑箱）而让其输出0（不是同一个人）或1（是同一个人），这样能学习到好的但不能显式表示的度量函数。再一次，这里可以通过freeze整个network的前大半部分，而precompute特征向量，只train这个黑箱，来加速网络的训练
- Neural Style Transfer
  - 如何visualize一个conv net的每一层到底detect了什么：可以选出使该层的某些activation unit的值达到最大值的输入图片来get some sense
  - Neural Style Transfer的关键就是定义一个cost function，由两部分组成：content cost和style cost。content cost评价结果图片和content image的“相似”程度，style cost评价结果哦图片和style image的style的“相似”程度。
  - 计算这两种cost的方法都是：选择某个层，对content/style input image和结果图片分别计算该层的输出，然后在该层上定义content/style的相似性，然后对所有层的content/style相似性加权求和便得到最终的content/style相似性。唯一的区别是content/style的相似性的定义：对于content相似性直接用对应层的值求差和平方和即可，对于style相似性要先计算该层上C个channel间的correlation得到一个CxC的矩阵然后再用两个图片的这个矩阵求差和平方和得到（为啥是这样呢？correlation可以这样理解：correlate的意思是，对应于两个不同channel的两个特征要么同时出现要么同时不出现）
  - 对于每层在最终相似性中的权重，可以通过第一步的visualization得到一些sense。

# 我对计算机视觉的一些理解

- 拿灰度图像来说，如果用0-255来编码一个灰度图像，很自然我们用0表示黑255表示白，数值越大越接近白，越小越接近黑
- 为什么不能吧顺序打乱？例如0表示浅灰，100表示白，200表示黑，255表示深灰色等等？
- 原因是这种编码表示必须满足一个特性：如果我们肉眼看到两个灰度很接近，那么这两个灰度在编码上的表示也应该很“接近”
- 怎么定义“接近”？这里用的是差值：差值越小越相似
- 那我可不可以用其他相似性定义？我觉得可以，只要这种定义满足线性关系（例如a和b比a和c更相似，那么a和b的相似性的数学表示应该在值上和a和c的相似值满足某种全序关系。问：可以是偏序吗？）但是除了正常的线性编码外（例如0表示黑255表示白或者反过来），这样的相似性定义应该很难找
- 而两个图像相似是一个至少是一个三元关系：颜色相似、形状相似且大小相似。所以我对视觉计算的理解就是如何定义、计算并处理这些相似性。
- 这就决定了为什么convolution用的是乘法和加法：待续。。。

# 其他资源

- [MSRA计算机视觉的修炼秘笈](https://www.msra.cn/zh-cn/news/features/book-recommendation-cv)
- [计算机视觉入门书](https://www.zhihu.com/question/28813777)
- [科学空间](https://kexue.fm/)
