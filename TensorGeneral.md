**TensorFlow编程模式**

> https://www.zhihu.com/question/49909565/answer/250388930

基于 Symbolic 编程， 将计算过程抽象为计算图，所有输入节点、运算节点和输出节点均符号化处理。

**TensorFlow基本概念**

- 使用图（Graph）表示计算流程
- 在会话（Session）中执行图
- 使用张量（Tensor）表示数据
- 使用变量（Variable）维护状态
- 使用feed和fetch为任意的操作赋值或从中获取数据



**Graph**

graph表示计算流程, 节点称为操作（Operation，简称OP）。每个OP接受0到多个Tensor，执行计算，输出0到多个Tensor。图是对计算流程的描述，**需要在Session中运行**。Session将计算图的OP分配到CPU或GPU等计算单元，并提供相关的计算方法，并且会返回OP的结果。



**Senssion**

 在会话（Session）中执行图

**Tensor**

TF使用Tensor表示所有数据，相当于Numpy中的ndarray，0维的数值、一维的矢量、二维的矩阵到n维数组都是Tensor。相对于Numpy，TensorFlow提供了创建张量函数的方法，以及导数的自动计算。

方法类似于 numpy

```
a= tf.zeros((2,2))
tf.reduce_sum(a,reduction_indices=[1]) 
a.getshape
```

**Variable**

在训练模型时，Variable被用来存储和更新参数。Variable包含张量储存在内存的缓冲区中，必须显式地进行初始化，在训练后可以写入磁盘。

```
import tensorflow as tf
# Create a Variable
state = tf.Variable(0, name="counter")
# Create an Op to add one to `state`.
one = tf.constant(1)
new_value = tf.add(state, one)
update = tf.assign(state, new_value)
# Variables must be initialized by running an `init` Op after having# launched the graph.  We first have to add the `init` Op to the graph.
init_op = tf.global_variables_initializer()
# Launch the graph and run the ops.
with tf.Session() as sess:
# Run the 'init' op
    sess.run(init_op)
# Print the initial value of 'state'
    print(sess.run(state))
# Run the op that updates 'state' and print 'state'.
    for _ in range(3):
        sess.run(update)
        print(sess.run(state))
```

代码到 run 位置才真正被计算。

**Feed**

使用Variable和Constant引入数据外，还提供了Feed机制实现从外部导入数据。一般Feed总是与占位符placeholder一起使用。

**Fetch**

要获取操作的输出，需要执行会话的run()函数，并且提供需要提取的OP。下面是获取输出的典型例子：

```
input1 = tf.constant([3.0])
input2 = tf.constant([2.0])
input3 = tf.constant([5.0])
intermed = tf.add(input2, input3)
mul = tf.mul(input1, intermed)
with tf.Session() as sess:
	result = sess.run([mul, intermed]) 
	print(result)
```

**图的构建**

构建图的第一步，是创建源OP(source op)，源操作不需要任何输入，例如常量(constant)，源操作的输出被传递给其它操作做运算。

Python库中，OP构造器的返回值代表被构造出的OP的输出，这些返回值可以传递给其它OP构造器作为输入。

```
import tensorflow as tf
# Create a Constant op that produces a 1x2 matrix.  The op is
# added as a node to the default graph.
# The value returned by the constructor represents the output# of the Constant op.
matrix1 = tf.constant([[3., 3.]])
matrix2 = tf.constant([[2.],[2.]])
multiplication.product = tf.matmul(matrix1, matrix2)
```

**图的执行**

要执行计算图，首先需要创建Session对象，如果不提供参数，Session构造器将运行默认图。可以用 with 控制语句自动关闭会话

```
with tf.Session() as sess:
 	result = sess.run([product])
	print(result)
```

