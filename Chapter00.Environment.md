# AI Environment

## Tensorflow

1. `pip install tensorflow`
2. test tensorflow
   ```py
    import tensorflow as tf
    hello=tf.constant('hello, tf')
    sess=tf.Session()
    print(sess.run(hello))
   ```