python 运行速度很慢，tensorflow 在训练时调用的是用 C++ 写好的模块。python 的开发速度很快。调用tensorflow 的 python api 时，实际上是定义了模型的流程图，或者曰计算图 (graph)。由此定义了数据的流动方向和权重优化方案之后，在一个中 session 向图喂入数据，不断计算 graph 和调整权重。

所以在 tf 中使用 python 的方式和存粹的 python 不近相同，包括数据的声明（都快变成强类型语言了）与传入（sess.run(op, feed={}) 与作为函数参数传入），图的计算（a=b+c 与 a = tf.add(b,c) ），全局变量(tf.collection)与 summary 等。特别是在引入了 tf.contrib 等高级 api 后，给计算图的定义带来了许多便利与~~不便~~。


# 图的计算: a = b + c, a = tf.add(b, c)

# session 中数据的传入

[read TFRecord](http://warmspringwinds.github.io/tensorflow/tf-slim/2016/12/21/tfrecords-guide/)

# tf 是如何知道哪些参数需要调整、计算梯度的？

# 数据声明与全局变量
[Losses](https://www.tensorflow.org/api_guides/python/contrib.losses)

[Losses with weight](https://www.tensorflow.org/api_docs/python/tf/losses/compute_weighted_loss)

# 梯度的优化与分布式

# tf.app 

# tf.collection

# model_deploy 
[model_deploy.py](https://github.com/tensorflow/models/blob/master/slim/deployment/model_deploy.py)

# model restore?
[A quick complete tutorial to save and restore Tensorflow models](http://cv-tricks.com/tensorflow-tutorial/save-restore-tensorflow-models-quick-complete-tutorial/)

