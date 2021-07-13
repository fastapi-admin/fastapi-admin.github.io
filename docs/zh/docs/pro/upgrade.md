# 从开源版本升级

从开源版本升级到Pro版本非常简单，因为Pro版本拥有与开源版本相同的项目结构并且包含所有特性。

1. 卸载开源版本。

    ```shell
    > pip uninstall fastapi-admin
    ```

2. 安装Pro版本。

    ```shell
    > pip install git+https://${GH_TOKEN}@github.com/fastapi-admin/fastapi-admin-pro.git
    ```

这就是所有需要的操作，然后你可以自由增加Pro版本独有的特性。

你也可以参考 [示例代码](https://github.com/fastapi-admin/fastapi-admin-pro/tree/dev/examples) 。
