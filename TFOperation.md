**Variable 和 constant**

**使用 Variable**

```
state = tf.Variable(0, name='counter')
init = tf.global_variables_initializer()
```

在 TF中设定变量必须用上面的 init 进行初始化， 但是init 这操作要在 sess.run(init)才会被激活

**初始化Tensor**

```
a=tf.constant(2) 
```

**进行运算**

首先要launch graph, 才能进行运算， with 的作用是自动控制 session 开始结束

```
with tf.Session() as sess:
    print "a: %i" % sess.run(a), "b: %i" % sess.run(b)
    print "Addition with constants: %i" % sess.run(a+b)
```

**Placeholder**

TensorFlow's feed mechanism lets you inject data into any Tensor in a computation graph. 值被放在feed_dict={}并且意义对应

```
a = tf.placeholder(tf.int16)
b = tf.placeholder(tf.int16)
add = tf.add(a, b)
mul = tf.multiply(a, b)
#一样的此时 add,mul 并不会进行运算print add 会得到一个 Tensor
```

```
with tf.Session() as sess:
    print "Addition with variables: %i" % sess.run(add, feed_dict={a: 2, b: 3})
```

**同理可以进行正常矩阵运算**

```
matrix1 = tf.constant([[3., 3.]])
matrix2 = tf.constant([[2.],[3.]])
product=tf.matmul(matrix1,matrix2)
with tf.Session() as sess:
    result = sess.run(product)
    print result
```