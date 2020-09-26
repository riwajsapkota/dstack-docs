# Troubleshooting

## Pushing data doesn't work \(e.g. returns 400, 403, 500, or other HTTP errors\)

1. Try to enable logs:

{% tabs %}
{% tab title="Python" %}
```python
from dstack import logger

logger.enable()
```
{% endtab %}
{% endtabs %}

2. Try to push the data again

3. File an issue to [https://github.com/dstackai/dstack-py/issue](https://github.com/dstackai/dstack-py/issues). Make sure to attach the code that you use to push data, the version of the `dstack` package, and the contents of your `.dstack/logs` folder.  
If you push the data to the in-cloud version of dstack.ai, make sure to include the name of your user.

{% hint style="warning" %}
If you see something doesn't work as you expect, or you have a question, please make sure to immediately write about it to [team@dstack.ai](mailto:team@dstack.ai). Our team will be happy to help right away! 🙌
{% endhint %}

