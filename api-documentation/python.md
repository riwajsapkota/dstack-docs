# Python

{% hint style="info" %}
You can find the complete open source python implementation here -[https://github.com/dstackai/dstack-py](https://github.com/dstackai/dstack-py)
{% endhint %}

## Pushing Frames 

The `push_frame()` function create a frame in the stack, commits and pushes the data in a single operation. 

You can push **datasets**, **models** and **plots** and specify other optional parameters.

```python
push_frame(stack: str, 
           obj,
           description: Union[str, NoneType] = None,
           access: Union[str, NoneType] = None,
           message: Union[str, NoneType] = None,
           params: Union[Dict, NoneType] = None,
           encoder: Union[dstack.handler.Encoder[Any], NoneType] = None,
           profile: str = 'default', 
           **kwargs) -> dstack.stack.PushResult
```

{% tabs %}
{% tab title="Parameters" %}
**`stack`**: A stack you want to commit and push to.

**`obj`**: Object to commit and push, e.g. plot, DataFrame, etc.

**`description`**: Optional description of the object.ept HTTP 200, e.g. in the case of authorization         access: Access level for the stack. It may be public, private or None. It is None by default, so it will be default access level in user's settings.

**`message`**: Push message to describe what's new in this revision.

**`params`**: Optional parameters.

**`encoder`**: Specify a handler to handle the object, by default \`AutoHandler\` will be used.

**`profile`**: Profile you want to use, i.e. username and token. Default profile is 'default'
{% endtab %}

{% tab title="Raises" %}
**ServerException**: If server returns something except HTTP 200, e.g. in the case of authorization failure.

**ConfigurationException**: If something goes wrong with configuration process, config file does not exist an so on.
{% endtab %}
{% endtabs %}

## Pulling Frames

The `pull()` function create a frame in the stack, commits and pushes the data in a single operation.

```python
pull(stack: str,
     profile: str = 'default',
     params: Union[Dict, NoneType] = None,
     decoder: Union[dstack.handler.Decoder[~T], NoneType] = None, 
     **kwargs) -> ~T
```

{% tabs %}
{% tab title="Parameters" %}
**`stack`**: A stack you want to commit and push to.

**`profile`**: Profile you want to use, i.e. username and token. Default profile is 'default'.

**`decoder`**: Specify a handler for decoder.
{% endtab %}

{% tab title="Exception Raises" %}
**ServerException**: If server returns something except HTTP 200, e.g. in the case of authorization failure.

**ConfigurationException**: If something goes wrong with configuration process, config file does not exist an so on.
{% endtab %}
{% endtabs %}

