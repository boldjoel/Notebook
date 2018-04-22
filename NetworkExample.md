

# 整个神经网络包括

##整个模型的定义 

1.对网络的定义包括input，output， 网络结构

```
def add_layer(inputs, in_size, out_size, activation_function=None)
```
例子：简单的一层隐藏层的模型
```
# add hidden layer
l1 = add_layer(xs, 1, 10, activation_function=tf.nn.relu)
# add output layer
prediction = add_layer(l1, 10, 1, activation_function=None)
```
2. Loss function

```
loss = tf.reduce_mean(tf.reduce_sum(tf.square(ys - prediction),
                        reduction_indices=[1]))
```
3. 训练方法
```
train_step = tf.train.GradientDescentOptimizer(0.1).minimize(loss)
```

## 训练过程

参数的初始化

```
if int((tf.__version__).split('.')[1]) < 12:
    init = tf.initialize_all_variables()
else:
    init = tf.global_variables_initializer()
sess = tf.Session()
sess.run(init)
```

训练

```
for i in range(1000):
    # training
    sess.run(train_step, feed_dict={xs: x_data, ys: y_data})
    if i % 50 == 0:
        # to see the step improvement
        print(sess.run(loss, feed_dict={xs: x_data, ys: y_data}))
```

