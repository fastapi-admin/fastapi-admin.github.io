# 独占内容

## 登录验证码

你可以在管理员登录界面设置验证码，只需要设置`enable_captcha=True`。

```python3
login_provider = UsernamePasswordProvider(user_model=User, enable_captcha=True)
```

## Google Recaptcha V2

除了图片验证码，你还可以设置`Google Recaptcha V2`来保护你的登录界面。

`GoogleRecaptcha` 类：

```python
class GoogleRecaptcha(BaseModel):
    cdn_url: str = "https://www.google.com/recaptcha/api.js"
    verify_url: str = "https://www.google.com/recaptcha/api/siteverify"
    site_key: str
    secret: str
```

然后在`LoginProvider`设置`google_recaptcha`参数。

```python
from fastapi_admin.providers.login import GoogleRecaptcha

await admin_app.configure(
    providers=[
        LoginProvider(
            google_recaptcha=GoogleRecaptcha(
                site_key=settings.GOOGLE_RECAPTCHA_SITE_KEY,
                secret=settings.GOOGLE_RECAPTCHA_SECRET,
            ),
        ),
    ]
)
```

## 登录失败IP限制

如果你需要在管理员多次登录失败后限制其IP以防止密码爆破，你可以使用`LoginPasswordMaxTryMiddleware`。

```python
admin_app.add_middleware(BaseHTTPMiddleware, dispatch=LoginPasswordMaxTryMiddleware(max_times=3, after_seconds=360))
```

## 权限控制

`PermissionProvider` 允许你通过管理员角色对每一项资源设置增删改查权限。

## 额外文件上传

### ALiYunOSS

支持阿里云OSS文件上传。

### AwsS3

支持AWS S3文件上传。

## 系统维护

如果你的站点维护，你可以设置展示处于维护状态中。

```python
await admin_app.configure(maintenance=True)
```

## 管理日志

如果你需要记录所有的管理员操作日志，你可以增加配置`AdminLogProvider`。

```python
await admin_app.configure(providers=[AdminLogProvider(Log)])
```

## 站点搜索

启用站点搜索。

```python
await admin_app.configure(providers=[SearchProvider()])
```

## 通知

启用websocket实时通知。

```python
await admin_app.configure(providers=[NotificationProvider()])
```

## OAuth2

当前内置支持`GitHubOAuth2Provider` 和 `GoogleOAuth2Provider`两种方式。

```python
await admin_app.configure(
    providers=[
        GitHubProvider(Admin, settings.GITHUB_CLIENT_ID, settings.GITHUB_CLIENT_SECRET),
        GoogleProvider(
            Admin,
            settings.GOOGLE_CLIENT_ID,
            settings.GOOGLE_CLIENT_SECRET,
            redirect_uri="https://fastapi-admin-pro.long2ice.io/admin/oauth2/google_oauth2_provider",
        ),
    ]
)
```
