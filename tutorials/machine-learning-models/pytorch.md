# PyTorch

Let's consider an example in Pytorch where we define our own new Model with a class.

### 1. Creating the Model

```python
import torch
from dstack import push_frame
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

### 2. Pushing the model

```python
# finally push the model
push_frame("my_torch_model", model, "My first PyTorch model")        
```

We stored only model weights, so to pull it we should provide model class to decoder, because `pull` method is not smart enough to guess which particular class to use. The following example shows a common pattern how to use pull in this case:

### 3. Pulling the Model with a Decoder Parameter

```python
from dstack.torch.handlers import TorchModelWeightsDecoder
from dstack import pull

my_model = pull("my_torch_model", decoder=TorchModelWeightsDecoder(LinearRegression(1, 1)))
```

