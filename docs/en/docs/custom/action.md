# Extra Actions

There are three kinds of action, `toolbar action` show in right top of table header, `action` show in end of table
row, `bulk action` show in left top of table header.

![](https://raw.githubusercontent.com/fastapi-admin/fastapi-admin/dev/images/actions.png)

The format request link of `Action` is `/{resource}/{action_name}/{pk}`, `BulkAction`
is `/{resource}/{action_name}` which request params is `pks` as a list of pk,`ToolbarAction`
is `/{resource}/{action_name}`
.

## Action

```python
class Action(BaseModel):
    icon: str
    label: str
    name: str
    method: enums.Method = enums.Method.POST
    ajax: bool = True

    @validator("ajax")
    def ajax_validate(cls, v: bool, values: dict, **kwargs):
        if not v and values["method"] != enums.Method.GET:
            raise ValueError("ajax is False only available when method is Method.GET")

```

## ToolbarAction

```python
class ToolbarAction(Action):
    class_: Optional[str]
```
