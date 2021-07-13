# 入门指南

`FastAPI-Admin` 很容易能集成到已有的`FastAPI`应用，只需要很少的配置。

## 挂载

首先，你需要挂载admin app到已存在的`FastAPI`应用。

```python
from fastapi_admin.app import app as admin_app
from fastapi import FastAPI

app = FastAPI()
app.mount("/admin", admin_app)
```

## 配置

需要在`FastAPI`的`startup`事件中进行初始化配置。

```python
from fastapi_admin.app import app as admin_app
from fastapi_admin.providers.login import UsernamePasswordProvider
import aioredis
from fastapi import FastAPI

login_provider = UsernamePasswordProvider(user_model=User, enable_captcha=True)

app = FastAPI()


@app.on_event("startup")
async def startup():
    redis = await aioredis.create_redis_pool("redis://localhost", encoding="utf8")
    admin_app.configure(
        logo_url="https://preview.tabler.io/static/logo-white.svg",
        login_logo_url="https://preview.tabler.io/static/logo.svg",
        template_folders=[os.path.join(BASE_DIR, "templates")],
        login_provider=login_provider,
        maintenance=False,
        redis=redis,
    )
```

所有的配置项可以在 [配置](/reference/configuration)找到。

## 定义并注册资源

有三种类型的资源，`Link`、`Model`和`Dropdown`。

### Link

`Link` 可以展示自定义界面或者跳转到第三方界面。

```python
from fastapi_admin.app import app
from fastapi_admin.resources import Link


@app.register
class Home(Link):
    label = "Home"
    icon = "ti ti-home"
    url = "/admin"
```

### Field

`Field`在`Model`资源中使用，定义每一个字段如何展示和输入。

### Model

使用`Model`将一个`TortoiseORM`的`model`映射为一个资源菜单，可以进行增删改查。

```python

from examples.models import User
from fastapi_admin.app import app
from fastapi_admin.resources import Field, Model
from fastapi_admin.widgets import displays, filters, inputs


@app.register
class UserResource(Model):
    label = "User"
    model = User
    icon = "ti ti-user"
    page_pre_title = "user list"
    page_title = "user model"
    filters = [
        filters.Search(
            name="username", label="Name", search_mode="contains", placeholder="Search for username"
        ),
        filters.Date(name="created_at", label="CreatedAt"),
    ]
    fields = [
        "id",
        "username",
        Field(
            name="password",
            label="Password",
            display=displays.InputOnly(),
            input_=inputs.Password(),
        ),
        Field(name="email", label="Email", input_=inputs.Email()),
        Field(
            name="avatar",
            label="Avatar",
            display=displays.Image(width="40"),
            input_=inputs.Image(null=True, upload_provider=upload_provider),
        ),
        "is_superuser",
        "is_active",
        "created_at",
    ]
```

### Dropdown

`Dropdown` 展示为一个下拉菜单，可以嵌套包含`Link`和`Model`。

```python


from examples import enums
from examples.models import Category, Product
from fastapi_admin.app import app
from fastapi_admin.resources import Dropdown, Field, Model
from fastapi_admin.widgets import displays, filters


@app.register
class Content(Dropdown):
    class CategoryResource(Model):
        label = "Category"
        model = Category
        fields = ["id", "name", "slug", "created_at"]

    class ProductResource(Model):
        label = "Product"
        model = Product
        filters = [
            filters.Enum(enum=enums.ProductType, name="type", label="ProductType"),
            filters.Datetime(name="created_at", label="CreatedAt"),
        ]
        fields = [
            "id",
            "name",
            "view_num",
            "sort",
            "is_reviewed",
            "type",
            Field(name="image", label="Image", display=displays.Image(width="40")),
            "body",
            "created_at",
        ]

    label = "Content"
    icon = "ti ti-package"
    resources = [ProductResource, CategoryResource]
```

### 下一步？

基本配置已完毕，现在你可以启动你的App。更多可参考[Reference](/zh/reference)。
