# 安装

## 从 pypi安装

```shell
> pip install fastapi-admin
```

## 从源码安装

```shell
> pip install git+https://github.com/fastapi-admin/fastapi-admin.git
```

### 使用 requirements.txt

添加以下行。

```
-e https://github.com/fastapi-admin/fastapi-admin.git@develop#egg=fastapi-admin
```

### 使用 poetry

添加以下内容到 `[tool.poetry.dependencies]`。

```toml
fastapi-admin = { git = 'https://github.com/fastapi-admin/fastapi-admin.git', branch = 'develop' }
```
