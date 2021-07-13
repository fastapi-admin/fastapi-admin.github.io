# 安装

由于Pro版本不会发布到pypi，所以你不能直接进行安装。

## 依赖

为了从程序中访问，例如从命令行或者GitHub Actions，你需要创建一个个人访问令牌。

1. 访问 <https://github.com/settings/tokens>。
2. 点击 `Generate a new token`。
3. 输入名称以及限定域。
4. 生成令牌然后妥善保存。

## 使用 pip

```shell
> pip install git+https://${GH_TOKEN}@github.com/fastapi-admin/fastapi-admin-pro.git
```

## 使用 poetry

Add the following line in section `[tool.poetry.dependencies]`.

```toml
fastapi-admin-pro = { git = 'https://${GH_TOKEN}@github.com/fastapi-admin/fastapi-admin-pro.git' }
```

## 使用 requirements.txt

Add the following line.

```shell
-e https://${GH_TOKEN}@github.com/fastapi-admin/fastapi-admin-pro.git#egg=fastapi-admin-pro
```
