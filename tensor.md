# Tensorflow 入门



启动 TensorFlow 	

```
$ cd targetDirectory
$ source ./bin/activate 
$ python

```

网络的定义

### Convolutional Layer

**size 的问题**:  对于conv2d（二维卷积单通道）filter 的个数=output 的channel 数

Output shape=input -filter shape+2*pad/stride +1

用 tf.layers.conv2d 创建二维 filter 的卷积层, 其中参数分别为

tf.layers.conv2d（inputs=输入,filter=通道数,kernel_size=filter 的大小,padding=填充方法，activation=激活函数 ）

```
conv1 = tf.layers.conv2d(
    inputs=input_layer,
    filters=32,
    kernel_size=[5, 5],
    padding="same",
    activation=tf.nn.relu)
```

### Pooling Layer

```
 pool1 = tf.layers.max_pooling2d(inputs=conv1, pool_size=[2, 2], strides=2)
```

###Fully Connected Layer

tf.reshape(input ,[-1])** : special case, used to flatten input, 用于后面全连接（FC层）

```
# tensor 't' is [[[1, 1, 1],
#                 [2, 2, 2]],
#                [[3, 3, 3],
#                 [4, 4, 4]],
#                [[5, 5, 5],
#                 [6, 6, 6]]]
# tensor 't' has shape [3, 2, 3]
# pass '[-1]' to flatten 't'
reshape(t, [-1]) ==> [1, 1, 1, 2, 2, 2, 3, 3, 3, 4, 4, 4, 5, 5, 5, 6, 6, 6]
```

对于分类任务， 会加入一层 tf.argmax用于预测， 用 tf.nn.softmax 层来生成概率和计算 loss 这两层是平行的input 是一样的

## 定义完网络后：

- 定义 loss 用于 train，evaluation用于评估训练得到的模型的效果

  比如在 MNIST 中用 cross entropy 来计算 loss

- Configure train op

  ​

![WechatIMG223](/Users/Joel/Dropbox/DTNotebook/imgs/WechatIMG223.jpeg)



## Main 函数

- 导入dataset，包括 train data和 evaluate data（input, output）

- Estimator 导入定义好的网络结构

```
  mnist_classifier = tf.estimator.Estimator(
      model_fn=cnn_model_fn, model_dir="/tmp/mnist_convnet_model")
```

- train 的参数 和实际运行 network