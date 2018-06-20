# 性能提升方法
    1. 小模型 mobilenet
    2. 模型压缩：参数稀疏、量化、分解、去冗余。
    3. 高性能计算。

# 模型压缩

[DeepCompression-caffe](https://github.com/Ewenwan/DeepCompression-caffe)


## 为什么要压缩网络？
    做过深度学习的应该都知道，NN大法确实效果很赞，
    在各个领域轻松碾压传统算法，
    不过真正用到实际项目中却会有很大的问题：

    计算量非常巨大；
    模型特别吃内存；
    
    这两个原因，使得很难把NN大法应用到嵌入式系统中去，
    因为嵌入式系统资源有限，而NN模型动不动就好几百兆。
    所以，计算量和内存的问题是作者的motivation；

## 如何压缩？
      论文题目已经一句话概括了：
        Prunes the network：只保留一些重要的连接；
        Quantize the weights：通过权值量化来共享一些weights；
        Huffman coding：通过霍夫曼编码进一步压缩；
        
## 效果如何？
    Pruning：把连接数减少到原来的 1/13~1/9； 
    Quantization：每一个连接从原来的 32bits 减少到 5bits；

## 最终效果： 
    - 把AlextNet压缩了35倍，从 240MB，减小到 6.9MB； 
    - 把VGG-16压缩了49北，从 552MB 减小到 11.3MB； 
    - 计算速度是原来的3~4倍，能源消耗是原来的3~7倍；

## 网络压缩(network compression)
    尽管深度神经网络取得了优异的性能，
    但巨大的计算和存储开销成为其部署在实际应用中的挑战。
    有研究表明，神经网络中的参数存在大量的冗余。
    因此，有许多工作致力于在保证准确率的同时降低网路复杂度。

### 1、低秩近似。
     用低秩矩阵近似原有权重矩阵。
     例如，可以用SVD得到原矩阵的最优低秩近似，
     或用Toeplitz矩阵配合Krylov分解近似原矩阵。

### 2、剪枝(pruning) 在训练结束后，可以将一些不重要的神经元连接
    (可用权重数值大小衡量配合损失函数中的稀疏约束)或整个滤波器去除，
    之后进行若干轮微调。实际运行中，神经元连接级别的剪枝会
    使结果变得稀疏，
    不利于缓存优化和内存访问，有的需要专门设计配套的运行库。
    相比之下，滤波器级别的剪枝可直接运行在现有的运行库下，
    而滤波器级别的剪枝的关键是如何衡量滤波器的重要程度。
    例如，可用卷积结果的稀疏程度、该滤波器对损失函数的影响、
    或卷积结果对下一层结果的影响来衡量。
### 3、量化(quantization)。对权重数值进行聚类，
    用聚类中心数值代替原权重数值，配合Huffman编码，
    具体可包括标量量化或乘积量化。
    但如果只考虑权重自身，容易造成量化误差很低，
    但分类误差很高的情况。
    因此，Quantized CNN优化目标是重构误差最小化。
    此外，可以利用哈希进行编码，
    即被映射到同一个哈希桶中的权重共享同一个参数值。

### 4\


## Deep Compression 方法，包含
    裁剪，
    量化，
    编码 三个手段。

## 模型参数分析：
    网络中全连层参数和卷积层weight占绝大多数，
    卷积层的bias只占极小部分。
    而参数分布在0附近，近似高斯分布。




参数压缩针对卷积层的weight和全连层参数。
每一层的参数单独压缩。