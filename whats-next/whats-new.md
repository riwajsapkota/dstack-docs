# What's New

### dstack 0.6.a1

* `Application` is a brand new type of stacks. It allows Python developers to quickly build and publish applications and ML models.
  * It supports [interactive controls](../concepts/controls.md) \(check-boxes, combo-boxes, text fields, sliders, etc\).
  * It supports [interactive outputs](../concepts/outputs.md) \(plots, tables, markdown\).
  * It supports the built-in [caching mechanism](../concepts/caching.md).
  * It supports managing application [dependencies](../concepts/dependencies.md) \(local modules and packages as well as dependencies to third-party PyPi libraries\).
  * It supports writing application [logs](../concepts/logging.md).
  * See [quickstart](../quickstart.md) and [tutorials](../tutorials/) for more details.
* In `Settings`, it is now possible to manage users.
* The `Sharing` options now include the following settings:
  * The stack can be accessed via its link by anyone \(`public`\)
  * The stack can be accessed via its link by registered users \(`internal`\)
  * The stack can be accessed by selected users only \(`private`\)
* `dstack` is now possible to run either locally via [`pip`](https://pypi.org/project/dstack/) or via [Docker](https://hub.docker.com/repository/docker/dstackai/dstack). _Warning, the conda and CRAN packages are not going to be supported anymore._
* [dstack.cloud](https://dstack.cloud) is the new version of the free cloud version of dstack that supports publishing `Applications` and `Models`. _Warning, the old version of the cloud_ [_dstack.ai_](https://dstack.ai) _is going to be discontinued on 15th of Feburary._
* The `Reports` and `Jobs` are not supported anymore.
* The `Stacks` page is now split into `Applications` and `Models` pages.
* The `Charts` and `Datasets` stack types are not supported anymore.

