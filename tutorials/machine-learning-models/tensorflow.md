---
description: >-
  Build a simple neural network that classifies images using Keras, and using
  dstack to push and pull the model.
---

# Tensorflow

Here is a simple example where we build a model to classify Fashion MNIST images and push it to dstack. Both `tf.keras.Sequential` and `tf.keras.Model` are supported.

Note: Only version 2 of Tensorflow in supported by dstack.

{% hint style="info" %}
You can find the open source implementation of Tenorflow for dstack on [Github here](https://github.com/dstackai/dstack-py/blob/aec55ca60e72cfefc78b4e05eebbcca2bb31219c/dstack/tensorflow/handlers.py). 
{% endhint %}

### 1. Import, building and training the model

```python
from dstack import push
import tensorflow as tf

d = 30

#Loading and splitting the dataset for training and testing
fashion_mnist = tf.keras.datasets.fashion_mnist
(train_images, train_labels), (test_images, test_labels) = fashion_mnist.load_data()


#Building the Neural Network model 
model = tf.keras.Sequential([
    tf.keras.layers.Flatten(input_shape=(28, 28)),
    tf.keras.layers.Dense(128, activation='relu'),
    tf.keras.layers.Dense(10)
])

#Compiling the model
model.compile(optimizer='adam',
              loss=tf.keras.losses.SparseCategoricalCrossentropy(from_logits=True),
              metrics=['accuracy'])
              
# Training the model for 10 epochs
pulled_model.fit(train_images, train_labels, epochs=10)
```

This will result in the following looking output if you don't have the dataset downloaded already.

```text
Downloading data from https://storage.googleapis.com/tensorflow/tf-keras-datasets/train-labels-idx1-ubyte.gz
32768/29515 [=================================] - 0s 1us/step
Downloading data from https://storage.googleapis.com/tensorflow/tf-keras-datasets/train-images-idx3-ubyte.gz
26427392/26421880 [==============================] - 5s 0us/step
Downloading data from https://storage.googleapis.com/tensorflow/tf-keras-datasets/t10k-labels-idx1-ubyte.gz
8192/5148 [===============================================] - 0s 0us/step
Downloading data from https://storage.googleapis.com/tensorflow/tf-keras-datasets/t10k-images-idx3-ubyte.gz
4423680/4422102 [==============================] - 1s 0us/step
Epoch 1/10
1875/1875 [==============================] - 2s 1ms/step - loss: 3.5043 - accuracy: 0.7218
Epoch 2/10
1875/1875 [==============================] - 3s 1ms/step - loss: 0.6577 - accuracy: 0.7700
Epoch 3/10
1875/1875 [==============================] - 2s 1ms/step - loss: 0.5774 - accuracy: 0.7930
Epoch 4/10
1875/1875 [==============================] - 2s 1ms/step - loss: 0.5266 - accuracy: 0.8108
Epoch 5/10
1875/1875 [==============================] - 3s 2ms/step - loss: 0.5186 - accuracy: 0.8167: 0s - loss: 0.5213 - accuracy
Epoch 6/10
1875/1875 [==============================] - 3s 2ms/step - loss: 0.4859 - accuracy: 0.8285
Epoch 7/10
1875/1875 [==============================] - 3s 2ms/step - loss: 0.4889 - accuracy: 0.8303
Epoch 8/10
1875/1875 [==============================] - 3s 2ms/step - loss: 0.4742 - accuracy: 0.8369
Epoch 9/10
1875/1875 [==============================] - 2s 1ms/step - loss: 0.4666 - accuracy: 0.8372
Epoch 10/10
1875/1875 [==============================] - 3s 1ms/step - loss: 0.4597 - accuracy: 0.8417
<tensorflow.python.keras.callbacks.History at 0x7fb7ef01ea90>
```

### 2. Pushing the Model

Now finally we can push the model to our stack with the model name and description. 

Note: The model  we are pushing is of type `tensorflow.python.keras.engine.sequential.Sequential`

```python
# push the model
push("my_tf_model", model, "My first TF model")
```

### 3. Pulling the Model

To pull model you need simply call `pull`, because the model is standard, no additional information \(i.e. decoder informatiion\) required.

You can read the [API Documentation](../../api-documentation/python.md#pulling-frames) to see what parameters you can specify when pulling the model.

```python
from dstack import pull
pulled_model = pull("my_tf_model")
```

### 4. Using the Model

Because of dstack saving the model, we can re-use the model that we `pull` and use it for other prediction tasks, making plots, or anything else. We can quite simply just evaluate the accuracy of the downloaded model like this as an example 

Now we can evaluate the accurary of the model.

```python
# Calling the .evaluate() method on the model we pulled from the stack
test_loss, test_acc = pulled_model.evaluate(test_images,  test_labels, verbose=2)
print('\nTest accuracy:', test_acc)
```

This will result in the follow

```text
313/313 - 0s - loss: 0.5459 - accuracy: 0.0993

Test accuracy: 0.09929999709129333
```



