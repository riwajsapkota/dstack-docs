---
description: >-
  Build a simple neural network that classifies images using Keras, and using
  dstack to push and pull the model.
---

# Tensorflow

Here is a simple example where we build a model to classify Fashion MNIST images and push it to dstack. Both `tf.keras.Sequential` and `tf.keras.Model` are supported for Te

Note: Only version 2 of Tensorflow in supported by dstack.

## 1. Import, building and training the model

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

## 2. Pushing the Model

Now finally we can push the model to our stack with the model name and description.

Note: The model we are pushing is of type `tensorflow.python.keras.engine.sequential.Sequential`

```python
# push the model
push("my_fmnist_model", model, "My Fashion MNIST TF model description")
```

## 3. Pulling the Model

To pull model you need simply call `pull`, because the model is standard, no additional information \(i.e. decoder information\) required.

You can read the [API Documentation](../../api-documentation/python.md#pulling-frames) to see what parameters you can specify when pulling the model.

```python
from dstack import pull
pulled_model = pull("my_fmnist_model")
```

## 4. Using the Model

Because of dstack saving the model, we can re-use the model that we `pull` and use it for other prediction tasks, making plots, or anything else. We can quite simply just evaluate the accuracy of the downloaded model or just see check out the weights of the model like this -

```python
pulled_model.get_weights()
```

This will result in the following output.

```text
[array([[ 0.052427  , -0.02945182,  0.0617029 , ...,  0.00246967,
          0.01090038, -0.07755718],
        [-0.05723156,  0.03596468, -0.02651855, ..., -0.07887074,
          0.01080904,  0.02068436],
        [-0.02881298, -0.01288668,  0.01784912, ...,  0.0607804 ,
          0.08622854, -0.00698646],
        ...,
        [-0.00706109, -0.02201258,  0.05412888, ...,  0.03179949,
          0.03583328,  0.04675704],
        [ 0.06938776,  0.02186611,  0.05148546, ...,  0.03782473,
          0.01074666,  0.0092198 ],
        [ 0.04965598,  0.06793301,  0.05037758, ..., -0.04798691,
          0.06559014,  0.02190205]], dtype=float32),
 array([-1.08301295e-02, -2.93479823e-02, -3.31198946e-02, -2.27169096e-02,
        -3.30042467e-02,  6.98878586e-01,  1.41454092e-03, -6.26472151e-03,
        -4.30478975e-02, -1.12457741e-02, -2.72011254e-02, -3.20343152e-02,
         1.81194901e-01, -3.65844741e-02,  8.64663720e-03, -2.06819791e-02,
        -4.69237715e-01,  5.18687360e-04,  2.12540776e-02, -4.08993363e-02,
        -1.84886664e-01, -4.71102893e-02, -5.18001281e-02, -3.11401915e-02,
        -7.64497602e-03,  1.45663708e-01,  1.41210869e-01,  1.48645183e-02,
        -4.14492376e-02, -4.09594566e-01, -3.02634519e-02, -6.59465045e-02,
        -1.04467217e-02, -2.98853759e-02, -2.70633493e-02, -2.19291579e-02,
        -4.80433069e-02, -1.47551503e-02, -5.55402264e-02, -9.67813209e-02,
        -2.49205250e-02, -5.55978343e-02,  1.76925883e-02, -2.74522286e-02,
        -5.14411151e-01, -1.09147374e-02, -1.48895392e-02,  1.23206407e-01,
        -5.74666522e-02, -2.95850821e-02, -1.86985712e-02,  3.20994928e-02,
        -3.40254195e-02, -3.57674956e-02, -1.03820646e-02,  8.72572232e-03,
         9.14188102e-03,  2.38248289e-01,  3.93682510e-01, -2.29456536e-02,
        -6.67976663e-02, -3.73756923e-02, -7.87850618e-02, -3.38005722e-02,
        -5.08897044e-02, -2.75799148e-02, -2.90932488e-02, -2.28850860e-02,
        -3.34851146e-02,  4.92599495e-02, -4.14704420e-02, -4.82968353e-02,
        -3.90044861e-02, -4.71138544e-02, -1.48571255e-02, -9.69296205e-04,
        -2.09787246e-02, -6.23180941e-02,  3.70422423e-01, -1.68444272e-02,
        -1.17933480e-02, -4.57984433e-02, -2.83229351e-02, -2.11163033e-02,
        -2.56153625e-02,  4.06454593e-01, -1.28762368e-02, -3.68343517e-02,
        -4.53493670e-02, -3.62827666e-02, -3.55754048e-02,  4.57404694e-03,
        -1.47999795e-02, -3.32705714e-02,  2.86874790e-02,  6.70332462e-02,
        -5.74343242e-02, -2.79022735e-02, -2.30609830e-02, -3.88930552e-02,
        -3.49004678e-02, -1.01527758e-03, -4.41650040e-02, -1.75610613e-02,
        -2.34310329e-02, -4.21381705e-02, -3.77351157e-02, -6.14695214e-02,
        -5.00598907e-01, -5.57847284e-02, -1.76530592e-02, -8.87354370e-03,
        -6.93210121e-03, -3.21005508e-02, -2.10604016e-02, -3.03169470e-02,
        -3.12260427e-02,  7.08136568e-03, -4.49887551e-02, -7.91136697e-02,
        -7.29015395e-02,  6.44802988e-01, -7.83587340e-03, -3.95041071e-02,
        -1.38829425e-02,  1.94236301e-02, -2.74980739e-02, -3.30666862e-02],
       dtype=float32),
 array([[ 0.12601511, -0.2006994 ,  0.08926196, ...,  0.03735346,
          0.19127643, -0.13831161],
        [ 0.02368195,  0.06460873,  0.1085185 , ..., -0.00830316,
         -0.11335552,  0.07864862],
        [-0.20575409,  0.19648553,  0.16378948, ...,  0.18793656,
         -0.09957372, -0.05059876],
        ...,
        [ 0.09823912, -0.11959952, -0.07162615, ...,  0.09301441,
         -0.10069652, -0.16274717],
        [ 0.10642483,  0.11474889, -0.08087761, ..., -0.14041159,
          0.12441584, -0.11013313],
        [ 0.06748329,  0.09357402, -0.1382139 , ...,  0.0089735 ,
          0.15533756, -0.05429778]], dtype=float32),
 array([ 0.7052931 , -1.629194  ,  0.6382556 ,  0.01729372, -0.9911736 ,
        -0.44355634,  1.9706568 , -0.84389013,  0.07124615, -2.1749625 ],
       dtype=float32)]
```

We can see that the weights after training and compiling are saved in the model and hence we can use it anywhere we'd like to make predictions, classify, create plots, etc.

