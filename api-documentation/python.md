---
description: This is the API Reference for using dstack in Python.
---

# Python

Please make sure you have followed the [Installation procedure](../open-source/installation.md) before referring to the API.

{% hint style="success" %}
You can find the complete open source python implementation here -[https://github.com/dstackai/dstack-py](https://github.com/dstackai/dstack-py)
{% endhint %}

## Push

The `push()` method **creates** **a frame** in the stack, **commits** and **pushes** the data in a single operation. If you want to use interactive plots, you can use the `create_frame()`, `commit()`, and then `push()`

{% hint style="warning" %}
`push_frame()` method is now deprecated and is replaced by `push()` 
{% endhint %}

You can push **datasets**, **models** and **plots** and specify other optional parameters.

```python
push(stack: str, 
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
`ServerException`: If server returns something except HTTP 200, e.g. in the case of authorization failure.

`ConfigurationException`: If something goes wrong with configuration process, config file does not exist an so on.
{% endtab %}
{% endtabs %}

## Pull

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

**`params`**: Optional parameters.

**`decoder`**: Specify a handler for decoder. See the [PyTorch Tutorial](../tutorials/machine-learning-models/pytorch.md#3-pulling-the-model-with-a-decoder-parameter) for an example.
{% endtab %}

{% tab title="Exceptions Raised" %}
`ServerException`: If server returns something except HTTP 200, e.g. in the case of authorization failure.

`ConfigurationException`: If something goes wrong with configuration process, config file does not exist an so on.
{% endtab %}
{% endtabs %}

## Frame \(previously Create Frame\)

This can be used for interactive plots. Creates a new stack frame. The method also checks access to specified stack. It returns a new stack frame. It returns a `StackFrame` object which contains the `commit()` and `push()` methods as discussed later which can be used to add to the created frame and push the frame finally to the stack.

{% hint style="warning" %}
 `create_frame()`method has now been deprecated and been replaced by the `frame()` method.
{% endhint %}

```python
frame(stack: str,
      profile: str = "default",
      access: Optional[str] = None,
      auto_push: bool = False,
      check_access: bool = True) -> StackFrame
```

{% tabs %}
{% tab title="Parameters" %}
**`stack`**: A stack you want to commit and push to.

**`profile`**: Profile you want to use, i.e. username and token. Default profile is 'default'.

**`access`**: Specify access level for this stack. It can be **private** - This means the stack will be visible only for the author. **public** - The stack will be accessible for everyone. **None** - Default access level will be used, one can find it in own settings on dstack server. If it is not specified default access level will be used.

**`auto_push`**: Tells the system to push frame just after the commit. It may be useful if you want to see result immediately. Default is False.

**`check_access`**: Check access to be sure about credentials before trying to actually push something. Default is `True`.
{% endtab %}

{% tab title="Exceptions Raised" %}
`ServerException`: If server returns something except HTTP 200, e.g. in the case of authorization failure.

`ConfigurationException`: If something goes wrong with configuration process, config file does not exist an so on.
{% endtab %}
{% endtabs %}

## Add \(previously Commit\)

The `add()` method is part of the `StackFrame` object which is returned by the `frame()`method and as the name suggests, it adds the object \(some data\) to the `StackFrame`.

The parameters associated with the data also allow you to create interactive plots when you use the `add()` method.

{% hint style="warning" %}
 `commit()` has now been deprecated to be replaced by `add()`
{% endhint %}

```python
add(self,
    obj: Any,
    description: Optional[str] = None,
    params: Optional[Dict] = None,
    encoder: Optional[Encoder[Any]] = None,
    **kwargs)
```

{% tabs %}
{% tab title="Parameters" %}
**`obj`**: A data to commit. Data will be preprocessed by the handler but dependently on `auto_push` mode will be sent to server or not. If `auto_push` is False then the data won't be sent. Explicit push call need anyway to process committed data. auto\_push is useful only in the case of multiple data objects in the stack frame, _e.g. set of plots with settings._

**`profile`**: Profile you want to use, i.e. username and token. Default profile is 'default'.

**`params`**: Parameters associated with this data, e.g. plot settings.
{% endtab %}
{% endtabs %}

