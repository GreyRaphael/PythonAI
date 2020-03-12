# AI Environment

## Tensorflow 2.x Introduction

常用框架:
- scikit-learn
  - no GPU acceleration
  - no deeplearning
- Caffe: for deeplearning, C++, no auto-gradient(自动求导)
- Keras: wrapper, deprecated
- Chainer
- MXNet: by Amazon
- PyTorch: 结合了Torch的前端api和Caffe2的后端
- TensorFlow: Theano→Tensorflow 1.x →Tensorflow 2.x

```py
# tensorflow 1.x
import tensorflow as tf

a=tf.constant(7)
b=tf.constant(10)
c=tf.add(a, b)
d=tf.constant('hello, tf')

sess=tf.Session()
value_of_c=sess.run(c)
print(value_of_c) # 17
print(sess.run(d)) # hello, tf
sess.close()
```

```py
# pytorch
import torch

a=torch.tensor(1.)
a=torch.tensor(3.)

c=torch.add(a, b)
print(c.item())
```

```py
# tensorflow 2.x 借鉴了大量Pytorch, easy to use
import tensorflow as tf

a=tf.constant(1.)
b=tf.constant(2.)
c=tf.add(a, b)
print(float(c))
```

TensorFlow 2.x Advantanges:
- 自动求导
- GPU加速
- 神经网络API

example: tf 2.x, auto-grad

```py
import tensorflow as tf 

x = tf.constant(1.)
a = tf.constant(2.)
b = tf.constant(3.)
c = tf.constant(4.)

with tf.GradientTape() as tape:
	tape.watch([a, b, c])
	y = a**2 * x + b * x + c

[dy_da, dy_db, dy_dc] = tape.gradient(y, [a, b, c])
print(dy_da, dy_db, dy_dc)
```

example: gpu acceleration

```py
import tensorflow as tf
import timeit


with tf.device('/cpu:0'):
	cpu_a = tf.random.normal([10000, 1000])
	cpu_b = tf.random.normal([1000, 2000])
	print(cpu_a.device, cpu_b.device)

with tf.device('/gpu:0'):
	gpu_a = tf.random.normal([10000, 1000])
	gpu_b = tf.random.normal([1000, 2000])
	print(gpu_a.device, gpu_b.device)

def cpu_run():
	with tf.device('/cpu:0'):
		c = tf.matmul(cpu_a, cpu_b)
	return c 

def gpu_run():
	with tf.device('/gpu:0'):
		c = tf.matmul(gpu_a, gpu_b)
	return c 


# warm up
cpu_time = timeit.timeit(cpu_run, number=10)
gpu_time = timeit.timeit(gpu_run, number=10)
print('warmup:', cpu_time, gpu_time)


cpu_time = timeit.timeit(cpu_run, number=10)
gpu_time = timeit.timeit(gpu_run, number=10)
print('run time:', cpu_time, gpu_time)
```

## Environment Configuration

tensorflow:
- CPU版本: `conda install tensorflow`
- GPU版本: `conda install tensorflow-gpu`

```py
# in ipyton
import tensorflow as tf

tf.test.is_gpu_available() # True表示gpu能用
```