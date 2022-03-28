# 介绍

[Convolutional Neural Networks for Sentence Classification](https://arxiv.org/abs/1408.5882)

文本分类问题，抽取文本特征，然后转化成固定维度的特征向量，训练分类器

TextCNN通过一维卷积来获取句子的**n-gram**特征表示

TextCNN对文本浅层特征的抽取能力很强，在短文本领域如搜索、对话领域专注于意图分类时效果很好，应用广泛，且速度快，一般是首选。

对长文本领域，TextCNN主要靠filter窗口抽取特征，在长距离建模方面能力受限，且对语序不敏感



什么是n-gram模型？

**N-Gram是一种基于统计语言模型的算法**。

它的基本思想是将文本里面的内容按照字节进行大小为N的滑动窗口操作，形成了长度是N的字节片段序列。

每一个字节片段称为gram，对所有gram的出现频度进行统计，并且按照事先设定好的阈值进行过滤，形成关键gram列表，也就是这个文本的向量特征空间，列表中的每一种gram就是一个特征向量维度。

该模型基于这样一种假设，第N个词的出现只与前面N-1个词相关，而与其它任何词都不相关，整句的概率就是各个词出现概率的乘积。这些概率可以通过直接从语料中统计N个词同时出现的次数得到。常用的是二元的Bi-Gram和三元的Tri-Gram。

[n-gram讲解](https://zhuanlan.zhihu.com/p/32829048)

# 预训练的 Word Embeddings

Word embedding 是NLP中一组语言模型和特征学习技术的总称。

一般我们都会选择one-hot编码来表示神经网路的输入

但是对于词语，one-hot编码的维度过大，所以需要embedding降低纬度

Embedding是通过网络学习而来的特征表达。简单说就是通过某种方法对原来的One-Hot单词表示的空间映射到另外一个空间：这个空间的单词向量不在是One-Hot形式即0-1表示某类，而是使用一个浮点型的向量表示；新的空间单词向量的维度一般会更小；语义上相近的单词会更加接近。

相当于模型训练one-hot编码，我们不关心output，只关心模型的权重，权重就是embedding。

[Tensorflow——word embedding](https://www.tensorflow.org/text/guide/word_embeddings)

# 卷积

相比于一般CNN中的卷积核，这里的卷积核的宽度一般需要个词向量的维度一样，卷积核的高度则是一个超参数可以设置，比如设置为2、3等。然后剩下的就是正常的卷积过程。

![image-20220324214711388](/Users/chenguanjin/Library/Application Support/typora-user-images/image-20220324214711388.png)

正常情况下，卷积核的size是h*k，h是我们自己设定的参数（1-10），短文本选小的，长文本选大的，另一个维度k是已经被embedding的长度所固定的

相当于二维卷积，向两个方向滑动，而一维卷积则只能向下滑动，每次滑动取得一点特征

![image-20220324214953151](/Users/chenguanjin/Library/Application Support/typora-user-images/image-20220324214953151.png)

整个过程从X1开始，从[x1:xh]到[x n-h+1,xn] ,最后建造了一个特征图

![image-20220324215121368](/Users/chenguanjin/Library/Application Support/typora-user-images/image-20220324215121368.png)

由于我们选择的h不一致，所以我们产生的特征c也不一致，这个问题，我们可以通过最大池化层来进行解决，对每个特征图，我们提取一个最大特征c max

# 其他

Dropout 0-0.5,特征图多，效果不好的时候，尝试加大dropout

注意dropout对test data的影响

