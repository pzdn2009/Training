# Training LeNet on MNIST with Caffe

## 准备

从Git获取代码，编译，设置安装目录为CAFFE_ROOT。

下载图片：get_mnist.sh
格式转化：create_mnist.sh

## LeNet：MNIST 的分类模型

LeNet：一个基于梯度下降的深度学习网络应用技术。

主要步骤：
1. 定义network
2. 定义data layer
3. 定义convolution layer
4. 定义pooling layer
5. 定义fully connected layer
6. 定义ReLU layer
7. 定义Loss layer

模型定义由**多个层定义**组成。详细定义：
$CAFFE_ROOT/examples/mnist/lenet_train_test.prototxt

```protobuf
name: "LeNet"

layer {
} *
```

### L：Data，mnist

```protobuf
layer {
  name: "mnist" #层的名称
  type: "Data" #数据层，对应的图形用八边形表示
  top: "data" #输出到data层
  top: "label" #输出到label层
  include {
    phase: TRAIN  #训练阶段使用此配置
  }
  transform_param {
    scale: 0.00390625 # 1/256，把输入像素灰度归一到【0,1)，灰度级[0,255]
  }
  data_param {
    source: "./examples/mnist/mnist_train_lmdb" #数据库所在目录
    batch_size: 64 #一次处理的大小
    backend: LMDB # LMDB 与 LMDB
  }
}
layer {
  name: "mnist"
  type: "Data"
  top: "data"
  top: "label"
  include {
    phase: TEST #测试阶段使用此配置
  }
  transform_param {
    scale: 0.00390625
  }
  data_param {
    source: "./examples/mnist/mnist_test_lmdb"
    batch_size: 100
    backend: LMDB
  }
}
```

### L：Convolution，conv1，conv2

```protobuf
layer {
  name: "conv1" #层名称 
  type: "Convolution" #类型，卷积层
  bottom: "data" #数据来源
  top: "conv1" #数据输出
  param {
    lr_mult: 1  #learning rate and decay multipliers for the filters，这里没有给出decay_mult
  }
  param {
    lr_mult: 2 # learning rate and decay multipliers for the biases
  }
  convolution_param {
    num_output: 20  #20个学习滤波器
    kernel_size: 5 #滤波器的核大小
    stride: 1  #每次滑动步数
    weight_filler { 
      type: "xavier" #xavier 算法权重填充器
    }
    bias_filler {
      type: "constant" #偏置填充器，默认为0
    }
  }
}

layer {
  name: "conv2"
  type: "Convolution"
  bottom: "pool1"
  top: "conv2"
  param {
    lr_mult: 1
  }
  param {
    lr_mult: 2
  }
  convolution_param {
    num_output: 50
    kernel_size: 5
    stride: 1
    weight_filler {
      type: "xavier"
    }
    bias_filler {
      type: "constant"
    }
  }
}

```

### L：Pooling，pool1

```protobuf
layer {
  name: "pool1"
  type: "Pooling"
  bottom: "conv1"
  top: "pool1"
  pooling_param {
    pool: MAX #最大池
    kernel_size: 2 #2*2的池化区域
    stride: 2 # step two pixels (in the bottom blob) between pooling regions
  } #没有重叠
}

layer {
  name: "pool2"
  type: "Pooling"
  bottom: "conv2"
  top: "pool2"
  pooling_param {
    pool: MAX
    kernel_size: 2
    stride: 2
  }
}
```

### L：InnerProduct，ip1，ip2

```profobuf
layer {
  name: "ip1" #层名称
  type: "InnerProduct" #内积层
  bottom: "pool2" #数据输入
  top: "ip1" #数据输出
  param {
    lr_mult: 1 #权重学习率
  }
  param {
    lr_mult: 2 #偏置学习率
  }
  inner_product_param {
    num_output: 500 #输出一维向量数量
    weight_filler {
      type: "xavier"
    }
    bias_filler {
      type: "constant"
    }
  }
}

layer {
  name: "ip2"
  type: "InnerProduct"
  bottom: "ip1"
  top: "ip2"
  param {
    lr_mult: 1
  }
  param {
    lr_mult: 2
  }
  inner_product_param {
    num_output: 10
    weight_filler {
      type: "xavier"
    }
    bias_filler {
      type: "constant"
    }
  }
}
```
### L：ReLU，relu1

```protobuf
layer {
  name: "relu1"
  type: "ReLU"
  bottom: "ip1"
  top: "ip1"
}
```

### L：Accuracy，accuracy

```protobuf
layer {
  name: "accuracy" #层名称
  type: "Accuracy" #精确率层
  bottom: "ip2" #数据输入
  bottom: "label" #标签数据输入
  top: "accuracy" #输出
  include {
    phase: TEST #TEST阶段使用
  }
}
```

### L：SoftmaxWithLoss，loss

```protobuf
layer {
  name: "loss" #层名称
  type: "SoftmaxWithLoss" #SoftmaxWithLoss
  bottom: "ip2" #数据输入
  bottom: "label" #标签数据输入
  top: "loss" #输出
}
```

## Solver定义

$CAFFE_ROOT/examples/mnist/lenet_solver.prototxt

```txt
# The train/test net protocol buffer definition
# 模型网络定义文件
net: "examples/mnist/lenet_train_test.prototxt"
# test_iter specifies how many forward passes the test should carry out.
# In the case of MNIST, we have test batch size 100 and 100 test iterations,
# covering the full 10,000 testing images.
test_iter: 100
# Carry out testing every 500 training iterations.
test_interval: 500
# The base learning rate, momentum and the weight decay of the network.
base_lr: 0.01
momentum: 0.9
weight_decay: 0.0005
# The learning rate policy
lr_policy: "inv"
gamma: 0.0001
power: 0.75
# Display every 100 iterations
display: 100
# The maximum number of iterations
max_iter: 10000
# snapshot intermediate results
snapshot: 5000
snapshot_prefix: "examples/mnist/lenet"
# solver mode: CPU or GPU
# solver的模式
solver_mode: CPU
```

## 训练和测试模型
```sh
cd $CAFFE_ROOT
./examples/mnist/train_lenet.sh

./build/tools/caffe train --solver=examples/mnist/lenet_solver.prototxt $@
```

可以看出关系：caffe --> solver -->netprotobuf --> LMDB + Layers*。
