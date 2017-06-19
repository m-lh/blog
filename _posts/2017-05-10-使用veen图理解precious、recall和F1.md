---
title: 使用veen图理解precious、recall和F1
permalinks: f1.html
layout: post
---
![wiki的珍贵的回忆](https://m-lh.github.io/blog/assets/images/wiki的珍贵的回忆.png)

# 使用Venn图理解precision、recall和F1


对于我来说，精确度，召回率这些东西，都看过好几次了，但是一直没有多少印象。准确的说是没有多少直观的印象(intuition)。

终于在做cs224n的asignment3时，看了一眼英文维基<https://en.wikipedia.org/wiki/Precision_and_recall>，被那一张[图片](https://en.wikipedia.org/wiki/File:Precisionrecall.svg)所吸引，终于对这两个参数有了一些比较深刻地了解。

首先说明，PR这两个参数主要是在评估模型的时候使用。我们有这么多的等待分类的样本（测试集），如下图。

![](https://m-lh.github.io/blog/assets/images/f1_1.png)

假设我们知道，图中左半部分是我们有用的类，右半部分当然是不需要的类。如果左右不分，比较均匀。貌似也不再需要这两个参数衡量直接使用，准确率衡量就可以了。现在我们很不均匀。如下图所示

![很不均匀](https://m-lh.github.io/blog/assets/images/f1_2.png)

如果我们的分类器可以把所有左半部分的类都分类成正的，右半部分都不要，那就是理想的情况了。现实就是这么残酷，我们只能让分类器画一个圆。圆圈里面是想要的圆圈外面是不要的，选到的就说积极positive，没有选到的，就说他消极怠工negetive

于是我们就有分的正确的类分的错误的类。

有关系的，我们把他选上。这就是真
没关系的，没有选上，这也是真
有关系的，没有选上，这就是假
没关系的，被选上了，这也是假
于是我们就有了T和F两种标记

然后标记组合一下，就有了常见的TP,TN,FP,FN如下图

![](https://m-lh.github.io/blog/assets/images/f1_3.png)

精确率precision 表示的就是，在你所有选择出的条目中，有多少是真正有用的，正确的。

![](https://m-lh.github.io/blog/assets/images/precision.png)

召回率就是在所有应该被选择到的条目中，真正有用的，正确的。

![](https://m-lh.github.io/blog/assets/images/recall.png)

另外，人们nlp中，喜欢这样表示一些参数

- N ：黄金标准分割的单词数
- e ：分词器错误标注的单词数
- c ：分词器正确标注的单词数

R = c / N
P = c / (c + e)

完成于2017年5月10日 21:35:11
