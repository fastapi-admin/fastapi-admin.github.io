# 安装

Because pro version won't publish to pypi, so you can't install from it.

## 依赖

In order to access the repository programmatically (from the command line or GitHub Actions workflows), you need to create a personal access token:

1. Go to <https://github.com/settings/tokens>.
2. Click on Generate a new token.
3. Enter a name and select the repo scope.
4. Generate the token and store it in a safe place.

## 使用 pip

```shell
> pip install git+https://${GH_TOKEN}@github.com/fastapi-admin/fastapi-admin-pro.git
```

## 使用 poetry

Add the following line in section `[tool.poetry.dependencies]`.

```toml
fastapi-admin-pro = { git = 'https://${GH_TOKEN}@github.com/fastapi-admin/fastapi-admin-pro.git'}
```

## 使用 requirements.txt

Add the following line.

```shell
-e https://${GH_TOKEN}@github.com/fastapi-admin/fastapi-admin-pro.git#egg=fastapi-admin-pro
```
