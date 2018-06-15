---
layout: post
title:  "Tensorflow Tutorial"
date:   2018-04-29 19:02:06 +0800
featured-img: tensorflow
categories: tensorflow
---
# Tensorflow Tutorial

## Install python and tensorflow

1. Input the flowing command to install python:

   ```sh
   sudo apt-get install python-dev python-pip
   ```

   ![python_install_command](../assets/img/posts/tensorflow_tutorial/python_install_command.png)

2. Input the flowing command to install tensorflow:

   ```shell
   export TF_BINARY_URL=https://storage.googleapis.com/tensorflow/linux/cpu/tensorflow-1.3.0-cp27-none-linux_x86_64.whl
   sudo pip install $TF_BINARY_URL
   ```

   ![tflearn](../assets/img/posts/tensorflow_tutorial/tflearn.png)

   ![tflearn](../assets/img/posts/tensorflow_tutorial/tensorflow_install_command.png)

3. Check whether the tensorflow is installed successfully:

   ```python
   python
   import tensorflow as tf
   print tf.__version__
   ```

   

   ![tf_version](../assets/img/posts/tensorflow_tutorial/tf_version.png)

   

## Make some configuration for the vim

```shell
vim ~/.vimrc
set ts=4
set nu
```

![vim_vimrc](../assets/img/posts/tensorflow_tutorial/vim_vimrc.png)

![vimrc_content](../assets/img/posts/tensorflow_tutorial/vimrc_content.png)



## Examples

### First example

```python
import tensorflow as tf

a=tf.constant([1.0,2.0])
b=tf.constant([3.0,4.0])
result=a+b
print result
```

![tf3_1](D:/githubPage/mynewsite/3/yoyoengineer.github.io/assets/img/posts/tensorflow_tutorial/tf3_1.png)![tf3_1_content](D:/githubPage/mynewsite/3/yoyoengineer.github.io/assets/img/posts/tensorflow_tutorial/tf3_1_content.png)

[Graph](https://www.tensorflow.org/api_docs/python/tf/Graph): Build the calculation process of the neural network, but do not perform the calculation.

![3_1_result_explanation](../assets/img/posts/tensorflow_tutorial/3_1_result_explanation.png)![graph](../assets/img/posts/tensorflow_tutorial/graph.png)

### Second example

```python
import tensorflow as tf

x = tf.constant([[1.0,2.0]])
w = tf.constant([[3.0],[4.0]])
y = tf.matmul(x,w)
print y
with tf.Session() as sess:
    print sess.run(y)
```

[Session](https://www.tensorflow.org/api_docs/python/tf/Session):

A class for running TensorFlow operations.

A `Session` object encapsulates the environment in which `Operation` objects are executed, and `Tensor` objects are evaluated.

![tf3_2](../assets/img/posts/tensorflow_tutorial/tf3_2.png)

![tf3_2_content](../assets/img/posts/tensorflow_tutorial/tf3_2_content.png)

Now, there are many warnings, and we can remove them by modifying the .bashrc file. First, we need to do is adding `export TF_CPP_MIN_LOG_LEVEL=2` to the end of this file. Then, we should input `source ~/.bashrc`, and execute this command to make the change that we just made effective. Finally, after we run tf3_2.py again, we can find that those warnings are disappearing now.

![clear_warings](../assets/img/posts/tensorflow_tutorial/clear_warings.png)

![bashrc_adding_content](../assets/img/posts/tensorflow_tutorial/bashrc_adding_content.png)

