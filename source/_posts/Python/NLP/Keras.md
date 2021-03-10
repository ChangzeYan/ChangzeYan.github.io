---
title: Keras
author: ChangzeYan
date: 2021-03-09 19:27:36
tags: NLP
categories: Python
cover:
---

## Keras 和 Tensorflow的版本对应关系

```
tensorflow 2.0 - keras 2.2.4
tensorflow 1.9 - keras 2.2.0 对应keras-transformer==0.31.0
tensorflow 1.8 - keras 2.1.6
tensorflow 1.5 - keras 2.1.4 
tensorflow 1.4 - keras 2.1.3
tensorflow 1.3 - keras 2.1.2
tensorflow 1.2 - keras 2.1.1

TensorFlow 1.14.0 + Keras 2.2.5 on Python 3.6.
TensorFlow 1.13.0 + Keras 2.2.4 on Python 3.6.
TensorFlow 1.12.0 + Keras 2.2.4 on Python 3.6.
TensorFlow 1.11.0 + Keras 2.2.4 on Python 3.6.
TensorFlow 1.10.0 + Keras 2.2.0 on Python 3.6.
TensorFlow 1.12.0 + Keras 2.2.4 on Python 2.
TensorFlow 1.11.0 + Keras 2.2.4 on Python 2.
```

## 保存和加载模型
参考：[Keras框架下的保存模型和加载模型](https://blog.csdn.net/tszupup/article/details/85198949)

1. 完整保存使用model.save()完整地保存整个模型，将Keras模型和权重保存在一个HDF5文件中，该文件将包含：模型的结构，模型的参数以及优化器参数：用于继续训练过程
```python
from __future__ import print_function
import numpy as np
from keras.models import Sequential
from keras.layers.core import Dense, Activation
from keras.optimizers import SGD
from keras.utils import np_utils
 
# 随机数种子，重复性设置
np.random.seed(1671)
 
# 网络结构和训练的参数
NB_EPOCH = 20
BATCH_SIZE = 128
VERBOSE = 1
NB_CLASSES = 10
OPTIMIZER = SGD()
N_HIDDEN = 128
VALIDATION_SPLIT = 0.2
RESHAPED = 784
 
 
# 加载数据
def load_data(path="mnist.npz"):
    f = np.load(path)
    x_train, y_train = f['x_train'], f['y_train']
    x_test, y_test = f['x_test'], f['y_test']
    f.close()
    return (x_train, y_train), (x_test, y_test)
 
# 调用函数加载数据
(x_train, y_train), (x_test, y_test) = load_data()
# 数据预处理
(x_train, y_train), (x_test, y_test) = load_data()
# 数据变形、类型转换及归一化
x_train = x_train.reshape(60000, 784).astype('float32') / 255
x_test = x_test.reshape(10000, 784).astype('float32') / 255
# 打印消息
print('Training samples:', x_train.shape)
print('Testing samples:', x_test.shape)
# 将类别转换为one-hot编码
y_train = np_utils.to_categorical(y_train, NB_CLASSES)
y_test = np_utils.to_categorical(y_test, NB_CLASSES)
 
# 定义网络结构
model = Sequential()
model.add(Dense(N_HIDDEN, input_shape=(RESHAPED, )))
model.add(Activation('relu'))
model.add(Dense(N_HIDDEN))
model.add(Activation('relu'))
model.add(Dense(NB_CLASSES))
model.add(Activation('softmax'))
 
# 打印模型概述信息
model.summary()
 
# 编译模型
model.compile(loss='categorical_crossentropy', optimizer=OPTIMIZER, metrics=['accuracy'])
 
# 训练模型
history = model.fit(x_train, y_train, batch_size=BATCH_SIZE, epochs=NB_EPOCH, verbose=VERBOSE,
					validation_split=VALIDATION_SPLIT)
 
# 评估模型
score = model.evaluate(x_test, y_test, verbose=VERBOSE)
print('Test score:', score[0])
print('Test accuracy:', score[1])
 
# 保存模型
model.save('my_model.h5')
```
加载：
```python
from keras.models import load_model

# 加载整个模型
model = load_model('my_model.h5')
 
# 训练模型
history = model.fit(x_train, y_train, batch_size=BATCH_SIZE, epochs=NB_EPOCH, verbose=VERBOSE,
					validation_split=VALIDATION_SPLIT)
 
# 评估模型
score = model.evaluate(x_test, y_test, verbose=VERBOSE)
```
2. 保存模型结构和权重
```python
from __future__ import print_function
import numpy as np
from keras.models import Sequential
from keras.layers.core import Dense, Activation
from keras.optimizers import SGD
from keras.utils import np_utils

# 定义网络结构
model = Sequential()
model.add(Dense(N_HIDDEN, input_shape=(RESHAPED, )))
model.add(Activation('relu'))
model.add(Dense(N_HIDDEN))
model.add(Activation('relu'))
model.add(Dense(NB_CLASSES))
model.add(Activation('softmax'))
 
# 打印模型概述信息
model.summary()
 
# 编译模型
model.compile(loss='categorical_crossentropy', optimizer=OPTIMIZER, metrics=['accuracy'])
 
# 训练模型
history = model.fit(x_train, y_train, batch_size=BATCH_SIZE, epochs=NB_EPOCH, verbose=VERBOSE,
					validation_split=VALIDATION_SPLIT)
 
# 评估模型
score = model.evaluate(x_test, y_test, verbose=VERBOSE)
print('Test score:', score[0])
print('Test accuracy:', score[1])
 
# 保存模型的结构
json_string = model.to_json()		# 方式1
open('model_architecture_1.json', 'w').write(json_string)
yaml_string = model.to_yaml()		# 方式2
open('model_arthitecture_2.yaml', 'w').write(yaml_string)

# 保存模型的权重
model.save_weights('my_model_weights.h5')
 
# 打印消息
print('训练和保存模型结构完成！！！')
```

加载结构：
```python
from keras.models import model_from_json
from keras.models import model_from_yaml

# 加载模型结构
model = model_from_json(open('model_architecture_1.json', 'r').read())
或：
model = model_from_yaml(open('model_arthitecture_2.yaml', 'r').read())

#  加载模型权重
model.load_weights('my_model_weights.h5')

```

