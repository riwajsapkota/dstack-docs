---
description: >-
  Defining a Linear Regression model and pulling with a dstack Decoder
  parameter.
---

# PyTorch

Let's consider an example in Pytorch where we define our own new Model with a class.

#### 1. Defining and Training the Model

```python
import torch
import dstack as ds
from dstack.torch.handlers import TorchModelEncoder

# define a new model
class LinearRegression(torch.nn.Module):
    def __init__(self, input_size, output_size):
        super(LinearRegression, self).__init__()
        self.linear = torch.nn.Linear(input_size, output_size)

    def forward(self, x):
        out = self.linear(x)
        return out

model = LinearRegression(1, 1)

# here you are training the model
for epoch in range(100):
    ...

# to avoid compatibility issues we will store only model weights   
TorchModelEncoder.STORE_WHOLE_MODEL = False
```

#### 2. Pushing the model

```python
# finally push the model
ds.push("my_torch_model", model, "My first PyTorch model")        
```

We stored only model weights, so to pull it we should provide model class to decoder, because `pull` method is not smart enough to guess which particular class to use. The following example shows a common pattern how to use pull in this case:

#### 3. Pulling the Model with a Decoder Parameter

You can also import the Decoder from `dstack.torch.handlers` like this -  `from dstack.torch.handlers import TorchModelWeightsDecoder`

```python
my_model = ds.pull("my_torch_model", decoder=TorchModelWeightsDecoder(LinearRegression(1, 1)))
```

That's it! Now we can use this pulled model for any task we'd like and also share it with the rest of the team to collaborate.

